# Requirements

Jovian has minimal [system requirements](#system-hardware-requirements), it is intended for powerful servers and/or grid-computers but also works on powerful PCs and laptops. It has one major software dependencies: [Miniconda](https://docs.conda.io/en/latest/miniconda.html). During the installation procedure you will be asked if you want to automatically install `miniconda` (if it is not already available).  
Additionally, it depends on the [following software](#software-requirements), but this isn't a problem for most Linux systems as this is usually pre-installed.  

{==Update to match the new toolkit -->==}
Any metagenomics analysis, Jovian included, depends on several [public databases that you have to download](#databases).

## System & hardware requirements


### Operating system

We have developed and tested the software for the following Linux distributions ("distro's"):

- Red Hat Enterprise Linux 7
- CentOS 7  
- Ubuntu 18.04.4 LTS 
- Ubuntu 20.04 LTS

Additionally, Jovian also works on [Windows Subsystem for Linux version 2 (Ubuntu)](https://docs.microsoft.com/en-us/windows/wsl/install-win10) but this functionality is currently not guaranteed.

We do expect Jovian to work on other Linux distro's, but cannot guarantee stability.


### User rights

Jovian does **not** require root and/or sudo rights. In fact, using root will most likely not work.  
It is however necessary to have both *read* and *write* access to the `/tmp` folder on your system.

This won't be a problem most of the time since the `/tmp` folder is usually free to read from and write to. However, it is best to check this with your system administrator(s).

### Required disk space

Jovian requires several public databases to be installed locally in order to run. The required disk-space for these databases is up to 200 GB.

Furthermore, the installation of Jovian itself and all of its components will require ~ 10 GB.  

This excludes the required disk space for your own data. We advise to have at least 300 GB of free disk space before installing Jovian and/or its components.


## Software requirements

In order to install all required dependencies, Jovian will need the following packages.  
These software packages are usually pre-installed on Linux systems.



Install the required packages: (requires root and/or sudo rights)

=== "RHEL/CentOS"
    ```bash
    sudo yum install git curl wget which bzip2 make -y
    ```

=== "Ubuntu"
    ```bash
    sudo apt install git curl wget bzip2 make -y
    ```

Software    |Install on RHEL/CentOS |Install on Ubuntu      |Website                                |  
--------    |---------------------- |--------------------   |-------                                |
**git**     |`yum install git -y`   |`apt install git -y`   |https://git-scm.com/downloads          |
**curl**    |`yum install curl -y`  |`apt install curl -y`  |https://curl.haxx.se/                  |
**wget**    |`yum install wget -y`  |`apt install wget -y`  |https://www.gnu.org/software/wget/     |
**bzip2**   |`yum install bzip2 -y` |`apt install bzip2 -y` |http://www.bzip.org/                   |
**make**    |`yum install make -y`  |`apt install make -y`  |https://www.gnu.org/software/make/     |
**which**   |`yum install which -y` |N/A                    |http://savannah.gnu.org/projects/which |

## Databases

Following is a list of the required databases that are necessary for Jovian.  
We strongly advise to use our [automatic installer as described here](BROKEN). The automatic installer will make sure you have all the required databases available on your system and adds all the required information to the applicable configuration files.  
However, it's still possible to [install the various databases manually](BROKEN).

|Database name|Link|Installation instructions|
|:---|:---|:---|
|`NCBI NT`| ftp://ftp.ncbi.nlm.nih.gov/blast/db/ | [link](BROKEN)|
|`NCBI Taxdump`| ftp://ftp.ncbi.nlm.nih.gov/pub/taxonomy/ | [link](BROKEN)|
|`NCBI New_taxdump`| ftp://ftp.ncbi.nlm.nih.gov/pub/taxonomy/new_taxdump/ | [link](BROKEN)|
|`Virus-Host interaction database`| http://www.genome.jp/virushostdb/note.html | [link](BROKEN)|
|`Latest Human Genome`[^1]| https://support.illumina.com/sequencing/sequencing_software/igenome.html | [link](BROKEN)|
|`MGKit taxonomy`| https://mgkit.readthedocs.io/en/latest/scripts/download-taxonomy.html | [link](BROKEN)


[^1]:
    We suggest the latest human genome because Jovian is intended for clinical samples. You can however use any reference you'd like, as [explained here](BROKEN).