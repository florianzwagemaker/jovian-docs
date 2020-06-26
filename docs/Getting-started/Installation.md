# Installation instructions

The installation process of Jovian is made to be as easy as possible.  
Depending on your preferences, the installation can be done automatically with a single command or through manual individual commands.  

For inexperienced users, we **strongly** recommend you to use the automated installer.

## Downloading the package

The command below will download the latest Jovian release, places it in a directory named `Jovian` and will automatically navigate to this newly made directory.

```bash
git clone https://github.com/DennisSchmitz/Jovian.git; cd Jovian
```


If you wish to use Jovian on a per-project basis (recommended), then use the snippet below and change `#!bash "<project>"` to the name of your desired project. This will automatically navigate you to the newly made directory.
```bash
dir="<project>"; git clone https://github.com/DennisSchmitz/Jovian.git $dir; cd $dir
```

## Automatic installation process (recommended)

The automated installer will go through the installation process of basic Jovian dependencies and will install them for you, as well as the required databases. You'll only have to go through this process once.  

The entire installation process will take (depending on the speed of your computer) about an hour or two. There are a few manual interactions where you will be asked to confirm an action or to make a choice of options.  

Once the installer is busy with downloading and installing the required databases then it's fine to leave your pc as this step will take a long time without manual interventions.

Start the installer with the following command:

```bash
bash jovian -id
```

You can skip the various confirmation-prompts by using the `-y` flag. This will make sure all installation steps will simply execute with the default settings without you having to confirm.

```bash
bash jovian -id -y
```


## Manual installation

!!! Warning "Please only perform a manual installation if you're an experienced Linux user."

### Installing (mini)conda

Download and run the latest (mini)conda installer with the command below:

```bash
wget -O miniconda.sh https://repo.continuum.io/miniconda/Miniconda3-latest-Linux-x86_64.sh; \
chmod +x miniconda.sh; sh miniconda.sh && \
rm miniconda.sh
```
Follow the instructions given by the miniconda installer, please verify wether your installation was successful with the following:
```bash
. ~/.bashrc; conda activate base
```
The "base" environment should be ready active now, this is shown as `#!bash "(base) user@host:"` in your terminal.  
If the snippet above gives an error then that means the conda installation went wrong somewhere, or a required setting is missing. Please type `#!bash conda init bash` in your terminal and try the snippet above again.  
If there's still an error, please try installing miniconda again.

### Installing the databases

First create the required `Jovian_helper` conda environment and activate it. You can do this by running the following snippet:

```bash
conda env create -f bin/envs/Jovian_helper_environment.yaml; \
conda activate Jovian_helper
```
{==Please note that this snippet must be ran for the location where Jovian was downloaded==}

The conda environment `Jovian_helper` should be active now. If this environment is not active then please try to activate it with `conda activate Jovian_helper`

#### Download and install the human genome

Navigate to the location on your system where you wish to download the human genome.  
For example: `/mnt/db/Human_genome`  

Use the following snippet in order to download the human genome, remove the Epstein-barr virus contig and index the updated genome:
```bash
a=$(grep -c ^processor /proc/cpuinfo); b=$(echo $((a-2)))
aws s3 --no-sign-request --region eu-west-1 sync s3://ngi-igenomes/igenomes/Homo_sapiens/NCBI/GRCh38/Sequence/WholeGenomeFasta/ ./
awk '{print >out}; />chrEBV/{out="EBV.fa"}' out=temp.fa genome.fa; head -n -1 temp.fa > nonEBV.fa
rm EBV.fa temp.fa
mv nonEBV.fa genome.fa
bowtie2-build --threads "${b}" genome.fa genome.fa
# >> Press enter <<
```

Simply copy this snippet, paste it in your terminal and press enter. The process of downloading and re-indexing the human genome will take roughly 30 minutes or longer, depending on your computer/server.

#### Download and install the BLAST NT database

Navigate to the location on your system where you wish to download the NCBI BLAST NT database.  
For example: `/mnt/db/NT_database`  

Use the following command to download and/or update the NCBI BLAST NT database:
```bash
perl ${CONDA_PREFIX}/bin/update_blastdb.pl --decompress nt
```

Depending on your download speed, this process can take (several) hours.

#### Download and install the NCBI TaxDB

Navigate to the location on your system where you wish to download the NCBI TaxDB.  
For example: `/mnt/db/taxdb`  

Use the following command to download and/or update the NCBI TaxDB database:
```bash
perl ${CONDA_PREFIX}/bin/update_blastdb.pl --decompress taxdb
```

depending on your download speed, this process can take several minutes.

#### Download and install the MGKit-LCA database

Navigate to the location on your system where you wish to download the MGKit database.  
For example: `/mnt/db/mgkit_database`  

Use the following snippet to download and/or update the MGKit database(s):
```bash
bash "${CONDA_PREFIX}"/bin/download-taxonomy.sh
rm taxdump.tar.gz
wget -O nucl_gb.accession2taxid.gz ftp://ftp.ncbi.nlm.nih.gov/pub/taxonomy/accession2taxid/nucl_gb.accession2taxid.gz
wget -O nucl_gb.accession2taxid.gz.md5 https://ftp.ncbi.nlm.nih.gov/pub/taxonomy/accession2taxid/nucl_gb.accession2taxid.gz.md5
md5sum -c nucl_gb.accession2taxid.gz.md5
# >> Press enter <<
```

If you get an error during this installation, then please deactivate and reactivate the conda environment with: `#!bash conda deactivate; conda activate Jovian_helper`

#### Download and install the Krona-LCA database

Navigate to the location on your system where you wish to download the Krona-LCA database.  
For example: `/mnt/db/krona_database`

Use the following commands to download and/or update the Krona-LCA database(s):
```bash
bash "${CONDA_PREFIX}"/opt/krona/updateTaxonomy.sh ./
bash "${CONDA_PREFIX}"/opt/krona/updateAccessions.sh ./
# >> Press Enter <<
```

#### Download the Virus-Host Interaction database

Navigate to the location on your system where you wish to download the Virus-Host Interaction database.  
For example: `/mnt/db/virus-Host_interaction`  

Use the following command to download and/or update the Virus-Host Interaction database:
```bash
wget -O virushostdb.tsv ftp://ftp.genome.jp/pub/db/virushostdb/virushostdb.tsv
```

#### Download and install the NCBI New_taxdump database

Navigate to the location on your system where you wish to download and install the New_taxdump database.  
For example: `/mnt/db/new_taxdump`  

Use the following snippet to download and/or update the NCBI New_taxdump database(s):
```bash
wget -O new_taxdump.tar.gz ftp://ftp.ncbi.nlm.nih.gov/pub/taxonomy/new_taxdump/new_taxdump.tar.gz
wget -O new_taxdump.tar.gz.md5 https://ftp.ncbi.nlm.nih.gov/pub/taxonomy/new_taxdump/new_taxdump.tar.gz.md5
tar -xzf new_taxdump.tar.gz
for file in *.dmp; do gawk '{gsub("\t",""); if(substr($0,length($0),length($0))=="|") print substr($0,0,length($0)-1); else print $0}' < ${file} > ${file}.delim; done
# >> Press Enter <<
```
