<p align="center">
    <img align="center" width="25%" src="Assets/img/Jovian%20logo%20notext.svg">
    <h1 align="center">
        Jovian
        <br>
        A user-friendly Viromics toolkit
    </h1>
</p>

<p align="center">
    <a href="https://github.com/DennisSchmitz/Jovian/releases">
        <img alt="Github release" src="https://img.shields.io/github/v/release/DennisSchmitz/Jovian?include_prereleases&style=flat-square">
    </a>
    <a href="https://www.gnu.org/licenses/agpl-3.0">
        <img alt="licence" src="https://img.shields.io/badge/License-AGPL%20v3-blue.svg?style=flat-square">
    </a>
    <a href="https://snakemake.readthedocs.io">
        <img alt="Snakemake Version" src="https://img.shields.io/badge/snakemake-5.20.1-brightgreen.svg?style=flat-square">
    </a>
</p>
<p align="center">
    For Citations, please use the following DOI:<br>
    <a href="https://doi.org/10.5281/zenodo.3666156">
        <img alt="Zenodo DOI" src="https://zenodo.org/badge/DOI/10.5281/zenodo.3666156.svg">
    </a>
</p>

<p align="center">
    See the documentation:<br>
    <a href="https://jovian.rivm-bioinformatics.com">
        <img alt="Jovian Docs" src="https://github.com/florianzwagemaker/jovian-docs/workflows/Docs/badge.svg?branch=master&">
    </a>
    <br>
    Or view an example notebook:<br>
    <a href="https://mybinder.org/v2/gh/DennisSchmitz/Jovian_binder/master?filepath=Notebook_report.ipynb">
        <img alt="Launch an example notebook" src="https://mybinder.org/badge_logo.svg">
    </a>
</p>

---

> We're still working on this website. Not everything is finished.  
> It's possible that you'll find unfinished pages or broken links throughout this site.
> If you're looking for information that we haven't made available on this site yet. Please let us know through a [github issue][GH_issues]. We'll respond as soon as we can.


!!! info "Important information"
    This documentation site will always correspond to the latest release version of Jovian.  
    We recommend to always use the latest version as we are constantly working on improvements.


## Quick download
Use the following command to download the latest release of Jovian and move to the newly downloaded `Jovian/` directory.

```bash
git clone https://github.com/DennisSchmitz/Jovian.git; cd Jovian
```

!!! tip "Tip: Use Jovian in individual project folders so you can easily keep track of multiple analyses"
    You can also use Jovian on a per-project basis. Replace `#!bash "<project>"` with the desired name of your project in the snippet below.
    
    ```bash
    dir="<project>"; git clone https://github.com/DennisSchmitz/Jovian.git $dir; cd $dir
    ```

Please see the [Requirements][requirements] and [Installation][installation] pages if you haven't used Jovian before.

## About Jovian

Jovian is a Viromics toolkit to automatically process raw NGS data from human clinical matrices (faeces, serum, etc.) into clinically relevant information.  
Jovian has three supported workflows:  

- **[Illumina based Metagenomics][ILM_meta_wf]**:  
  Includes (amongst other features) data quality control, assembly, taxonomic classification, viral typing, and minority variant identification (quasispecies).

- **[Illumina based Reference-alignment][ILM_ref_wf]**:  
  Includes (amongst other features) data quality control, alignment, SNP identification, and consensus-sequence generation.  

- **[Nanopore based Reference-alignment][ONT_ref_wf]**:  
  Includes (amongst other features) data quality control, alignment, SNP identification, and consensus-sequence generation.

[See more information about the supported workflows][WF_overview]{: .md-button .md-button--primary}

## Key features of Jovian

- **User-friendliness**:  
  Wetlab personnel can start, configure and interpret results via an interactive web-report. [Click here for an example report][example_report]{target=_blank}.  
  This makes doing Public Health analyses much more accessible and user-friendly since minimal command-line skills are required.


- **Audit trail**:  
  All pipeline parameters, software versions, database information and runtime statistics are logged.  

- **Portable**:  
  Jovian is easily installed on off-site computer systems and at back-up sister institutes. Allowing results to be generated even when the internal grid-computer is down (speaking from experience).  

## Features

The features of Jovian are specific to the workflow that you choose to run. However, all the workflows support _at least_ the following general features:

- Data quality control (QC) and cleaning.  
  - Including library fragment length analysis, useful for sample preparation QC.  
- Removal of human[^1] data (patient privacy).
- Presentation of all processes through an [interactive web report](#visualizations) including an audit trail.

[^1]:
  You can use [whichever reference you would like](../FAQ#i-dont-care-about-removing-the-human-data-i-have-samples-that-are-from-other-species-can-i-also-automatically-remove-that). However, Jovian is intended for human clinical samples. 

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
- Importantly, results of all processes listed above are presented via an [interactive web-report](#visualizations) including an audit trail.  

### Reference-alignment specific features

- All cleaned reads are aligned against the user-provided reference fasta.  
- SNPs are called and a consensus genome is generated.  
- Consensus genomes are filtered at the following coverage cut-off thresholds: 1, 5, 10, 30 and 100x.  
- A tabular overview of the breadth of coverage (BoC) at the different coverage cut-off thresholds is generated.  
- Alignments and visualized via `IGVjs` and allow manual assessment and validation of consensus genomes.  


## Visualizations

All data are visualized via an interactive web-report, [as shown here][example_report].
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

- A collation of interactive QC graphs via `MultiQC`.
- Breadth-Of-Coverage, in regards of the given reference sequence, reports per input sample in various coverage thresholds.
- An interactive alignment viewer (`IGVjs`) showing detailed read-alignment for each input sample.
  - Detailed alignment information.
  - Depth of coverage graph
  - GC content graph
  - Identified SNPs
- All SNPs are presented via interactive spreadsheet-like tables, allowing detailed analysis.

<!-- LINKS ON THIS PAGE -->

[requirements]: Getting-started/Requirements
[installation]: Getting-started/Installation

[WF_overview]: Workflows/Overview

[ILM_meta_wf]: Workflows/Illumina/Metagenomics
[ILM_ref_wf]: Workflows/Illumina/Reference
[ONT_ref_wf]: Workflows/Nanopore/Reference

[example_report]: https://mybinder.org/v2/gh/DennisSchmitz/Jovian_binder/master?filepath=Notebook_report.ipynb
[GH_issues]: https://github.com/DennisSchmitz/Jovian/issues