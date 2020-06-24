# Jovian - a user friendly genomics toolkit

For citations, please use the following DOI:  
[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.3666156.svg)](https://doi.org/10.5281/zenodo.3666156)  

[![License: AGPL v3](https://img.shields.io/badge/License-AGPL%20v3-blue.svg)](https://www.gnu.org/licenses/agpl-3.0) [![GitHub release (latest by date including pre-releases)](https://img.shields.io/github/v/release/DennisSchmitz/Jovian?include_prereleases)](https://github.com/DennisSchmitz/Jovian/releases) [![Snakemake](https://img.shields.io/badge/snakemake-≥5.17.0-brightgreen.svg?style=flat)](https://snakemake.readthedocs.io) 


{==Links throughout the site are currently (mostly) broken==}  
{==Gotta change this description to something more up-to-date==}


## Quick download
Use the following command to download the latest release of Jovian and move to the newly downloaded `Jovian/` directory.
```bash
git clone https://github.com/DennisSchmitz/Jovian.git; cd Jovian
```
<br>
You can also use Jovian on a per-project basis. Replace `<project>` with the desired name of your project in the snippet below.
```bash
dir="<project>"; git clone https://github.com/DennisSchmitz/Jovian.git $dir; cd $dir
```
Please see the [Requirements](Getting-started/Requirements.md) and [Installation](Getting-started/Installation.md) docs if you haven't used Jovian before.

## Introduction

Jovian is a Public Health toolkit to automatically process raw NGS data from human clinical matrices (faeces, serum, etc.) into clinically relevant information. It has two main components:

- Metagenomic: This performs, amongst others, taxonomic classification, virus typing and minority variant identification (quasispecies). See details below.
- Reference-alignment: Given a user-provided fasta reference, all data is aligned against it, and a consensus genome is generated at different coverage cut-off thresholds. See details here below.

Jovian has several features necessary for diagnostic usage:

- User-friendliness: Wetlab personnel can start, configure and interpret results via an interactive web-report. This makes doing Public Health analyses much more accessible and user-friendly since minimal command-line skills are required.
- Audit trail: All pipeline parameters, software versions, database information and runtime statistics are logged. See details below.
- Portable: jovian is easily installed on off-site computer systems and at back-up sister institutes. Allowing results to be generated even when the internal grid-computer is down (speaking from experience)

## Supported workflows

Illumina:

  - Metagenomics
  - Reference based
  
Nanopore:
  
  - Reference based

**More to come**

## Features


### General features

- Data quality control (QC) and cleaning.  
  - Including library fragment length analysis, useful for sample preparation QC.  
- Removal of human[^1] data (patient privacy).
- Removal of PCR-duplicates.  


[^1]:
  You can use [whichever reference you would like](BROKEN_LINK). However, Jovian is intended for human clinical samples. 

### Metagenomics specific features

- Assembly of short reads into bigger scaffolds (often full viral genomes).  
- Taxonomic classification:  
  - Every nucleic acid containing biological entity (i.e. not only viruses) is determined up to species level.  
  - Lowest Common Ancestor (LCA) analysis is performed to move ambiguous results up to their last common ancestor, which makes results more robust.  
- Viral typing:
  - Several viral families and genera can be taxonomically labelled at the sub-species level as described [here](#virus-typing).  
- Viral scaffolds are cross-referenced against the Virus-Host interaction database and NCBI host database.  
- Scaffolds are annotated in detail:  
  - Depth of coverage.  
  - GC content.  
  - Open reading frames (ORFs) are predicted.  
  - Minority variants (quasispecies) are identified.  
- Importantly, results of all processes listed above are presented via an [interactive web-report](#visualizations) including an [audit trail](#audit-trail).  

### Reference-alignment specific features

- All cleaned reads are aligned against the user-provided reference fasta.  
- SNPs are called and a consensus genome is generated.  
- Consensus genomes are filtered at the following coverage cut-off thresholds: 1, 5, 10, 30 and 100x.  
- A tabular overview of the breadth of coverage (BoC) at the different coverage cut-off thresholds is generated.  
- Alignments and visualized via `IGVjs` and allow manual assessment and validation of consensus genomes.  


## Visualizations

All data are visualized via an interactive web-report, [as shown here](#example-jovian-report).
The content shown in the interactive web-report changes depending on the data and method that was used to analyze the samples. 

### Visualizations for the Metagenomic analysis workflow(s)

- A collation of interactive QC graphs via `MultiQC`.  
- Taxonomic results are presented on three levels:  
  - For an entire (multi sample) run, interactive heatmaps are made for non-phage viruses, phages and bacteria. They are stratified to different taxonomic levels.  
  - For a sample level overview, `Krona` interactive taxonomic piecharts are generated.  
  - For more detailed analyses, interactive tables are included. Similar to popular spreadsheet applications (e.g. Microsoft Excel).  
    - Classified scaffolds  
    - Unclassified scaffolds (i.e. "Dark Matter")  
- Virus typing results are presented via interactive spreadsheet-like tables.  
- An interactive scaffold alignment viewer (`IGVjs`) is included, containing:  
  - Detailed alignment information.  
  - Depth of coverage graph.  
  - GC content graph.
  - Predicted open reading frames (ORFs).  
  - Identified minority variants (quasispecies).  
- All SNP metrics are presented via interactive spreadsheet-like tables, allowing detailed analysis.

### Visualizations for the Reference based analysis workflow(s)

- add stuff here


## Viral typing

{==This section is wrong (no longer done by hand), update when convenient==}

After a Jovian analysis is finished you can perform virus-typing (i.e. sub-species level taxonomic labelling). These analyses can be started by the command `bash jovian -vt [virus keyword]`, where `[virus keyword]` can be:  

Keyword | Taxon used for scaffold selection | Notable virus species
--------|-----------------------------------|----------------------
`NoV`   | Caliciviridae                     | Norovirus GI and GII, Sapovirus  
`EV`    | Picornaviridae                    | Enteroviruses (Coxsackie, Polio, Rhino, etc.), Parecho, Aichi, Hepatitis A 
`RVA`   | _Rotavirus A_                     | Rotavirus A  
`HAV`   | _Hepatovirus A_                   | Hepatitis A  
`HEV`   | _Orthohepevirus A_                | Hepatitis E  
`PV`    | Papillomaviridae                  | Human Papillomavirus  
`Flavi` | Flaviviridae                      | Dengue (work in progress)
`all`   | All of the above                  | All of the above