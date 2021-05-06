# BCB546 Final Project: Directory Organization

Group members: Ashenafi Beyi, Maggie Sodders, Marissa Roghair Stroud, and Dua Vang

Project completed in BCB 546 during the Spring 2021 semester. 

Paper: **A High-Throughput Method for Identifying Novel Genes That Influence Metabolic Pathways Reveals New Iron and Heme Regulation in _Pseudomonas aeruginosa_**. David G. Glanville, Caroline Mullineaux-Sanders, Christopher J. Corcoran, Brian T. Burger, Saheed Imam, Timothy J. Donohue, Andrew T. Ulijasz. *mSystems* Feb 2021, 6 (1) e00933-20; DOI: [10.1128/mSystems.00933-20](https://msystems.asm.org/content/6/1/e00933-20)

---

This document describes the organization of our GitHub repository and the files found within it. Additional details about each part of the analysis are listed next to the relevant files. 

---

## Main Folder

This directory contains the following:

*`Glanville-et-al_summary_2021.docx` and `Glanville-et-al_summary_2021.pdf` files: Introduces the original paper, explains the technical details of your replication of analyses and summarizes your replication of the original results. 
* `Glanville_et_al_2021.pdf`: The study whose analysis we replicated. 
* `Figure2E.png`, `Figure2F.png`, `Figure3A.png`, and `Figure3C.png`: ggplot figures, with names corresponding to the figure that was reproduced. 

---
## `Code` directory 
This directory contains the commented code for the replication.

* `01-InitialDataStructure.md`: Downloading, inspecting, and describing the data utilized in the study. Also contains links to the packages used for analysis. 
* `02-DataProcessing.md`: Processing the data to format them for the analysis. 
* `03-DataAnalysis.md`: Rerunning the analysis described in the manuscript using Bowtie2 and TSAS. 
* `04-ProcessingPart2.md`: Processing the data to format them for plotting with ggplot in R. 
* `05-ggplotFigures.Rmd`: Providing ggplot figures of the results.
* `06-PythonProcessing.ipynb` : Python code needed for 2F randoms sampling 



---
## `Data` directory 
This directory contains links to data necessary to run your code.

### Read files downloaded from the Sequence Read Archive 

These 4 files need unzipped before proceeding with the analysis.

* `SRR13258538.fastq.zip`: Read files from the E3 enrichment of the original population. 
* `SRR13258539.fastq.zip`: Read files from the E2 enrichment of the original population. 
* `SRR13258540.fastq.zip`: Read files from the E1 enrichment of the original population. 
* `SRR13258541.fastq.zip`: Read files from the original Library population. 
* `Metadata_SRAfiles.csv`: Metadata file with information about the previous 4 files, containing accession numbers, total bases sequenced per library, and file size information for each read file. 

### *P. aeruginosa* MPAO1 genome sequence and annotation files

* `MPAO1_genome.fasta`: Complete genome sequence of MPAO1.
* `MPAO1_annotation.gff`: Annotation of the MPAO1 genome, in `.gff` format for use in TSAS. 
* `MPAO1_feature_table.txt`: Annotation of the MPAO1 genome, in `.txt` format for use in figure creation. 

### Supplemental tables 

* `TableS1A.csv`: Supplemental data table used to make Figure 3C. Downloaded from the mSystems website.

### Modified data files

The following files have been modified from the original format they were downloaded as and sorted or organized for ease of use in figure creation. 

#### Outputs from TSAS

* `wig` files: These files can be opened on a genome browser application to view the number of mapped reads per base pair for the entire genome. 
* `essential gene` files: These files contains a list of all annotated genes in the genome along with all the statistics calculated for each gene from the data provided for a one-sample analysis 

#### Files created for plotting in ggplot

* `MPAO1_feature_table_genes.txt`: Creation of this file is described in `04-ProcessingPart2.md`.  
* `MPAO1_feature_table_abbrev.txt`: Creation of this file is also described in `04-ProcessingPart2.md`. It only contains the gene name, start/end position, and the strand the gene lies on. 
* `Ess538`, `Ess539`, `Ess540`, `Ess541`: Essential gene files, outputs from TSAS to be used for plotting in R.
* `EssGenesAll_Merge.txt`: This is the final file uploaded into Rmd file `05-ggplotFigures.Rmd` for plotting.
* `Fig2E_TSASResults.txt`: This is the txt file containing the numbers of unique insertions and the numbers of unique insertions within genes used in order to plot Figure 2E. 
* `2F TSAS Data.csv`: .csv file containing the numbers of mapped reads and unique insertions from random samplings of SRR13258540.fastq in order to plot Figure 2F. 
