
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
`illumina-metagenomics`<br>`illumina-meta`<br>`ilm-meta`<br>`im` | N/A | Specifies the "Illumina-Metagenomics" workflow to be executed.<br>Must be accompanied by the following flag(s): <br> `-i`/`--input` | 
`illumina-reference`<br>`illumina-ref`<br>`ilm-ref`<br>`ir` | N/A | Specifies the "Illumina-Reference" workflow to be executed.<br>Must be accompanied by the following flag(s): <br>`-i`/`--input`<br>`-ref`/`--reference` |
`nanopore-reference`<br>`nanopore-ref`<br>`nano-ref`<br>`ont-ref`<br>`nr` | N/A | Specifies the "Nanopore-Reference" workflow to be executed.<br>Must be accompanied by the following flag(s): <br>`-i`/`--input`<br>`-ref`/`--reference`<br>`-pr`/`--primers`.
`-i`    |[input directory] | This is the folder containing your input fastq files.<br>Default is `raw_data/`.<br>Both relative and absolute paths are accepted.  |
`-ref`<br>`--reference`   | [Reference fasta] | Path to the fasta file you wish to use as a reference. Must be a complete path.<br>Both relative and absolute paths are accepted.<br>Example: `/path/to/reference/file.fasta` |
`-pr`<br>`--primers`      | [Primers fasta]   | Only required in combination with the `nanopore-reference` workflow.<br>Path to the fasta file that contains all the primers used in order to produce the sequenced-amplicons.<br>Both relative and absolute paths are accepted.<br>Example: `/path/to/primer/file.fasta` |

#### Examples

!!! Example "Basic commands in order to start various analyses"
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
`--update`                | none, or version number | Updates the Jovian installation.<br>When no argument is specified, Jovian will update to the latest release.<br>When a version is specified, Jovian will "update" to that version. | 
`--archive`               | `-y`              | Archives all the output of a Jovian analysis into a single compressed file.<br>Using `-y` will skip the confirmation prompt. |
`--rebuild-archive`       | N/A               | Restores the data, results and logs from an archived Jovian run.<br>It's advised to use the same Jovian version for rebuilding the archive, as was used for building the archive.<br>Does not restore the databases. |
`--clean `                | `-y`              | Cleans the Jovian working directory. Removes all data, logs, and results.<br>Using `-y` will skip the confirmation prompt.<br>{==Removed data cannot be recovered.==}
`-id`<br>`--install-dabases`| `-y`            | Triggers the interactive installation procedure for the required databases.<br>Using `-y` will skip the confirmation prompt. Manual interaction is still required even when using `-y`.
`-ic`<br>`--install-dependencies` | N/A       | Installs the required virtual-environments and software dependencies that will be used by the Jovian workflow(s).<br>These environments are used for reproducibility of the analysis.
`--make-sample-sheet`     | N/A               | Only makes the sample sheet of the given input. Functions as a "debug only mode" as it allows the individual workflows to be executed without the various controllers included in Jovian.<br>Must be accompanied with a specific workflow and the input directory described [here](#starting-an-analysis). |
`-m`<br>`--mode`          | `strict` / `relaxed` | Changes the stringency settings for the various workflows where applicable.<br>Using `--mode` without any arguments displays the currently set analysis-mode.<br>Use `strict` for high precision at the cost of sensitivity. Use `relaxed` for high sensitivity at the cost of result-certainty.<br> Default: `relaxed` |
`--configure-jupyter` | N/A | Configures Jupyter Notebooks for use. This has to be done only once per user. | 
`--start-jupyter`     | N/A | Starts the Jupyter Notebook process. This keeps running until you manually stop the process. Provides you with a URL which you can use to view the interactive report(s). |
`-cq`<br>`--cluster-queue` | `<queue name>` | One-time override of the cluster-queue setting which is used when using Jovian in "HPC/grid mode". Use this command when you wish to use a different computing-queue without having to manually adjust the settings.<br>This does not change the default settings. |
`-sh`<br>`--snakemake-help` | N/A | Prints the snakemake help document.<br>Can't be combined with other commands |
`-k`<br>`--keep-going`      | N/A | Keep going with independent jobs if an analysis step fails.<br>Must be accompanied with a specific workflow and the input directory described [here](#starting-an-analysis) |
`-n`<br>`--dry-run`         | N/A | Test-run for a specified workflow, does not execute anything and displays what would be done.<br>Must be accompanied with a specific workflow and the input directory described [here](#starting-an-analysis) |
`-u`<br>`--unlock`          | N/A | Removes the lock on the working directory. Useful in case an analysis run ends abruptly and prevents you from doing a new analysis.<br>Must be accompanied with a specific workflow described [here](#starting-an-analysis) |
`-q`<br>`--quiet`           | N/A | Do not output any progress or job information.<br>Must be accompanied with a specific workflow and the input directory described [here](#starting-an-analysis) |
`vt-help`<br>`--virus-typing-help`  | N/A | Prints additional information about the virus-typing.<br>For example: which family/genus is selected and which species are typed. |
`-vt`<br>`--virus-typing`    | `all` / `NoV` / `EV` / `RVA` / `HAV` / `HEV` / `PV` / `Flavi` | {++Only compatible with the Jovian *Illumina-metagenomics* workflow++}<br>{++Under normal circumstances, you will not have to use this command. Virus typing is done automatically in the Jovian *Illumina-metagenomics* workflow since version `1.0.0`++}<br>After a Jovian *Illumina-metagenomics* workflow has completed, do viral typing for Noroviruses (`NoV`), Enteroviruses (`EV`), Hepatitis A viruses (`HAV`), Hepatitis E viruses (`HEV`), Rotavirus A viruses (`RVA`), Papillomaviruses (`PV`), or Flaviviruses (`Flavi`).<br>You can also use the `all` keyword to perform virus typing for all viruses listed above.<br>Can't be combined with other commands.<br>Use `-vt-help` for additional explanation. |
`-vt-force`<br>`--virus-typing-force` | `all` / `NoV` / `EV` / `RVA` / `HAV` / `HEV` / `PV` / `Flavi` | Same as `-vt` / `-virus-typing`, but overwrites existing output.<br>Can't be combined with other commands.<br>Use `-vt-help` for additional explanation. |
`--start-nginx` | N/A | {==Debug only==}<br>Starts the nginx process which is used by Jupyter notebook in order to stream data. |
`--stop-nginx`  | N/A | {==Debug only==}<br>Stops the nginx process which is used by Jupyter notebook in order to stream data. |

#### Examples

!!! note
    TBA