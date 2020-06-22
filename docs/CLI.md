{==Make this an actual table==}

This is a complete overview of all the commands that can be used in Jovian. Plus a few examples for good measure.

!!! attention
    This overview corresponds to Jovian version `1.1.0`  
    Previous versions will not be included in this overview, please reference `bash jovian -h` to view a list of possible commands for your specific Jovian version

!!! Info
    All commands and arguments must have `bash jovian` in front of it

## Commands

### Starting an analysis

Command         |Argument         |Explanation        |
---------       |---------        |---------          |
`illumina-metagenomics`<br>`illumina-meta`<br>`ilm-meta`<br>`im` | N/A | Specifies the "Illumina-Metagenomics" workflow to be executed. <br> Must be accompanied by the following flag(s): <br> `-i`/`--input` | 
`illumina-reference`<br>`illumina-ref`<br>`ilm-ref`<br>`ir` | N/A | Specifies the "Illumina-Reference" workflow to be executed. <br> Must be accompanied by the following flag(s): <br>`-i`/`--input`<br>`-ref`/`--reference` |
`nanopore-reference`<br>`nanopore-ref`<br>`nano-ref`<br>`ont-ref`<br>`nr` | N/A | Specifies the "Nanopore-Reference" workflow to be executed. <br> Must be accompanied by the following flag(s): <br>`-i`/`--input`<br>`-ref`/`--reference`<br>`-pr`/`--primers`
`-i`    |[input directory] | This is the folder containing your input fastq files. <br> Default is 'raw_data/'.<br>Both relative and absolute paths are accepted.  |
`-ref`<br>`--reference`   | [Reference fasta] | wutwut                                  |
`-pr`<br>`--primers`      | [Primers fasta]   | wowowow                                 |

!!! Example
    **Starting a metagenomics analysis:**
    ```bash
    bash jovian illumina-meta -i /path/to/raw/illumina-data/
    ```

    **Starting a reference-alignment analysis - Illumina data:**
    ```bash
    bash jovian illumina-ref -i /path/to/raw/illumina-data/ -ref /path/to/reference.fasta
    ```

    **Starting a reference-alignment analysis - Nanopore data:**
    ```bash
    bash jovian nanopore-ref -i /path/to/raw/nanopore-data/ -ref /path/to/reference.fasta -pr /path/to/used/primers.fasta
    ```

### General Jovian commands

Command                   |Arguments          |Explanation
----                      |----               |----
`-v`<br>`--version`       | N/A               | Displays the current version of Jovian<br>Can't be combined with other commands   |
`-h`<br>`--help`          | N/A               | Prints the Jovian help document<br>Can't be combined with other commands          |
`--update`                | none, or version number | Updates the Jovian installation.<br>When no argument is specified, then Jovian will update to the latest release.<br>When a specific version is specified, then Jovian will "update" to that version. | 
`--archive`               | `-y`              | Archives all the output of a Jovian analysis into a single compressed file.<br>Using `-y` will skip the confirmation prompt. |
`--rebuild-archive`       | N/A               | Restores the data, results and logs from an archived Jovian run.<br>It's advised to use the same Jovian version for rebuilding the archive, as was used for building the archive.<br>Does not restore the databases. |
`-id`<br>`--install-dabases`|


```
Jovian, Public Health Toolkit version v0.9.6.1-154-gd431fe1, built with Snakemake
  N.B. Current version only supports Illumina paired-end data, ONT compatibility is upcoming.
__________________________________________________________________________________________________
Use case 1, agnostic metagenomic analysis including virus typing (see below):
      bash jovian -i <INPUT_DIR> <parameters>
Use case 2, align against user-provided reference genome to generate a consensus genome:
      bash jovian -i <INPUT_DIR> -ra <REFERENCE_FASTA> <parameters>
__________________________________________________________________________________________________
Virus typing:
  -vt-help, --virus-typing-help     Print additional information about the virus-typing, i.e.
                                    which family/genus is selected and which species are typed.
  -vt, --virus-typing [all|NoV|EV|RVA|HAV|HEV|PV|Flavi]
                                    After a Jovian analyses has completed, do viral typing for
                                    Norovirus (NoV), Enterovirus (EV), Hepatitis A (HAV), 
                                    Hepatitis E (HEV), Rotavirus A (RVA), Papillomaviruses (PV),
                                    Flaviviruses (Flavi). Or use the 'all' keyword to perform
                                    virus typing for all above listed viruses.
                                    For additional explanation, --vt-help
  -vt-force, --virus-typing-force [NoV|EV|RVA|HAV|HEV|PV|Flavi]
                                    Same as above, but overwrites existing output. 
Parameters:
  -m, --mode [strict|relaxed]       Automatically configures Jovian to be stringent or relaxed
                                    in taxonomic classification of scaffolds. Please see the 
                                    publication for details. In short, use 'strict' for high 
                                    precision at the cost of sensitivity and use 'relaxed' for
                                    high sensitivity at the cost of more spurious results
                                    (contamination). Default: relaxed.
  -cq, --cluster-queue [Queue name] One-time override of the default cluster-queue settings.
                                    Use this command when you wish to use a different computing-
                                    queue without manually going through the jovian settings.
                                    This does not change your default settings.
  -sh, --snakemake-help             Print the Snakemake help document.
  --clean (-y)                      Removes all Jovian output, both the metagenomics output and
                                    reference alignment output. (-y forces "Yes" on all prompts)
  -k, --keep-going                  Useful snakemake command: Go on with independent jobs if
                                    a job fails.
  -n, --dry-run                     Useful snakemake command: Do not execute anything, and
                                    display what would be done.
  -u, --unlock                      Removes the lock on the working directory. This happens when
                                    a run ends abruptly and prevents you from doing subsequent
                                    analyses.
  -q, --quiet                       Useful snakemake command: Do not output any progress or
                                    rule information.
Archiving Jovian results:

Jupyter Notebook (for data visualization):
  --configure-jupyter               Sets the proper Jupyter settings. You only need to do this
                                    once per user.
  --start-jupyter                   Starts a Jupyter Notebook process. You must always have this
                                    running in a separate terminal (or in the background) if you
                                    want to open the final report with the interactive graphs
                                    and tables.
Installation:
  -ic, --install-dependencies       Install the required software dependencies for the
                                    metagenomics and reference alignment workflows.
  -id, --install-databases          Install required databases.
Debug only:
  --start-nginx                     Starts the nginx process.
  --stop-nginx                      Stops the nginx process.
  --make-sample-sheet               Only make the sample sheet.
‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾
```

