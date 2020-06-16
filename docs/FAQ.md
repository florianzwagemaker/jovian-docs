
## Frequently Asked Questions

??? Question "I get an error saying the directory is locked, what should I do?"
    It's most likely that an earlier analysis has crashed and/or was cancelled by the user while the pipeline was still running.  
    You can unlock the directory by typing `bash jovian --unlock`.

??? Question "Why are there multiple lines per taxid in the host/disease information table?"
    In the Virus-Host interaction database there are sometimes multiple entries for a single taxid, meaning, there are multiple known hosts. Therefore, we follow this formatting and print the different hosts on different lines.

??? Question "Why doesn't the virus typing-tool accept my query?"
    Please see [this](https://github.com/DennisSchmitz/Jovian/issues/29) and [this](https://github.com/DennisSchmitz/Jovian/issues/51) issue.  
    The short answer; they were made for Sanger sequences and are not yet able to to handle NGS datasets.  
    This is a work-in-progress.

??? Question "I am missing a certain taxa of which I'm sure is in the dataset. How is that possible?"
    Could be due to multiple reasons.  
    The first one being the stringency of the analysis: The current default values are quite strict, you might have filtered it away.  
    Please try more relaxed settings.  
    
    The second being the result of the LCA analysis (Lowest Common Ancestor) putting a certain scaffold at a unexpected taxonomic level.  
    Imagine a sequence that is homologous between (pro)phages and bacteria, the lowest common ancestor between phages and bacteria is the theoretical root of all life (i.e. `root` taxonomic level), so you will find it at the taxonomic level (you can try changing the `bitscoreDeltaLCA: 5` to `0` in the config-file.
    
    If it is anything other than these reasons, please let us know by making an [issue](https://github.com/DennisSchmitz/Jovian/issues).

??? Question "I don't care about removing the human data, I have samples that are from other species, can I also automatically remove that?"
    Yes.  

    Although we focus on patient-privacy since it was developed for clinical samples, you can enter any reference sequence you like.  
    During your first use of Jovian, you will be asked for the location of a *"background reference"* fasta, you can enter your preferred background reference sequence file there.  

    If you wish to change the background reference sequence file at a later stage, then reset your database choices with `bash jovian --reset-db`.  
    **For more advanced users**: You can manually edit the `~/.jovian_installchoice_db` file to change your background reference file.
    
    The only limitations are that it is a `fasta` file and that is indexed via `bowtie2`, although this latter process will be automated in a future version.  

??? Question "How can some scaffolds still be assigned to {==Homo sapiens==}? I thought Jovian removed human data?"
    The human genome is a consensus genome built from many individuals from around the globe.  
    It does not capture all diversity in the human gene pool and therefore cannot completely remove all human data.
    
    You can improve this by selecting a reference genome that is closer to your target population, e.g. if you sequence mainly Dutch samples, the [GoNL genome](http://www.nlgenome.nl/) might be better suited.  

    > We unfortunately cannot guarantee completely accurate removal of the Human Genome and will not take **any** responsibility when it comes to possible jurisdictional repercussions.

??? Question "Why does installing the pipeline take so long?"

??? Question "Why do you have to install the software for every analysis?"
    The answer to both these questions is the same; it is a consequence of making the analysis replicate-able and reproducible. Briefly, for the different analysis steps in the pipeline, disparate virtual environments are created and they take some time to build. Since these virtual environments are created using hard-coded recipes, we know which software was used, and users can easily revert to this environment using the Jovian Git hash (unique methodological fingerprint). Thereby allowing users to replicate your own (old) results or allowing other groups to reproduce your results (if you share the raw data with them). This also allows us developers to easily validate analyses, track and fix bugs and compare results between versions.

    Therefore, we recommend installing Jovian in a fixed location and only specifying different input directories. Once an analysis is finished, you can archive the results using `bash jovian --archive` and transfer them to back-up systems for long-term storage.

??? Question "In the scaffold viewer and the minority variant table, the DoC values of forward and reverse orientated reads do not add up to the value reported in the "Total_depth_of_coverage" column."
    Correct, please see [this link](https://www.biostars.org/p/46361/) for an explanation.

## Known Issues


!!! note
    TBA