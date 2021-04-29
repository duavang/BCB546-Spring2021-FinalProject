# BCB546 Final Project: Directory Organization

Group members: Ashenafi Beyi, Maggie Sodders, Marissa Roghair Stroud, and Dua Vang

Project completed in BCB 546 during the Spring 2021 semester. 

Paper: **A High-Throughput Method for Identifying Novel Genes That Influence Metabolic Pathways Reveals New Iron and Heme Regulation in _Pseudomonas aeruginosa_**. David G. Glanville, Caroline Mullineaux-Sanders, Christopher J. Corcoran, Brian T. Burger, Saheed Imam, Timothy J. Donohue, Andrew T. Ulijasz. *mSystems* Feb 2021, 6 (1) e00933-20; DOI: [10.1128/mSystems.00933-20](https://msystems.asm.org/content/6/1/e00933-20)

--

This document describes the organization of our GitHub repository and the files found within it. Additional details about each part of the analysis are listed next to the relevant files. 


_Note: To make organization of our "final" repository easier, I've placed text next to each file we need to upload with the words **DONE** or **NOT DONE**. Once all files are present in our GitHub repository, the text and this note can be deleted. - Marissa_

---



## Main Folder

This directory contains the following:

* An `author(s)-YEAR.md` file that introduces the original paper, explains the technical details of your replication of analyses and summarizes your replication of the original results. -- **NOT DONE**
* `Glanville_et_al_2021.pdf`: The study whose analysis we replicated. -- **DONE**
* `Figure2E.png`, `Figure2F.png`, `Figure3A.png`, `Figure3B.png`, and `Figure3C.png`: ggplot figures, with names corresponding to the figure that was reproduced. -- **NOT DONE**

---
## `Code` directory 
This directory contains the commented code for the replication.

* `01-InitialDataStructure.md`: Downloading, inspecting, and describing the data utilized in the study. -- **NOT DONE**
* `02-DataProcessing.md`: Processing the data to format them for the analysis. -- **NOT DONE**
* `03-DataAnalysis.md`: Rerunning the analysis described in the manuscript using Bowtie2 and TSAS. -- **NOT DONE**
* `04-ProcessingPart2.md`: Processing the data to format them for plotting with ggplot in R. -- **NOT DONE**
* `05-ggplotFigures.Rmd`: Providing ggplot figures of the results. -- **NOT DONE**


---
## `Data` directory 
This directory contains links to data necessary to run your code.

### Read files downloaded from the Sequence Read Archive 

These 4 files need unzipped before proceeding with the analysis.

* `SRR13258538.fastq.zip`: Read files from the E3 enrichment of the original population. -- **DONE**
* `SRR13258539.fastq.zip`: Read files from the E2 enrichment of the original population. -- **DONE**
* `SRR13258540.fastq.zip`: Read files from the E1 enrichment of the original population. -- **DONE**
* `SRR13258541.fastq.zip`: Read files from the original Library population. -- **DONE**
* `Metadata_SRAfiles.csv`: Metadata file with information about the previous 4 files, containing accession numbers, total bases sequenced per library, and file size information for each read file. -- **DONE**

### *P. aeruginosa* MPAO1 genome sequence and annotation files

* `MPAO1_genome.fasta`: Complete genome sequence of MPAO1. -- **DONE**
* `MPAO1_annotation.gff`: Annotation of the MPAO1 genome, in `.gff` format for use in TSAS. -- **DONE**
* `MPAO1_feature_table.txt`: Annotation of the MPAO1 genome, in `.txt` format for use in figure creation. -- **DONE**

### Supplemental tables 

_These tables need reformatted so they don't contain any extra columns/rows or extra information after the data lines end._

* `TableS1A.csv`: -- **NOT DONE**
* `TableS1B.csv`: -- **NOT DONE**
* `TableS1C.csv`: -- **NOT DONE**
* `...` -- **NOT DONE**


### Modified data files

The following files have been modified from the original format they were downloaded as and sorted or organized for ease of use in figure creation. 

#### Outputs from TSAS or PATRIC 

* `wig` files -- **NOT DONE**
* `essential gene` files -- **NOT DONE**
* other outputs, PATRIC files, etc... -- **NOT DONE**

#### Files created for plotting in ggplot

* `MPAO1_feature_table_genes.txt`: Creation of this file is described in `FILENAME.md`.  -- **DONE**
* `MPAO1_feature_table_abbrev.txt`: Creation of this file is also described in `FILENAME.md`. It only contains the gene name, start/end position, and the strand the gene lies on.  -- **DONE**
* `EssGenesAll_Merge.txt`: This is the final file uploaded into Rmd file `FILENAME` for plotting. -- **DONE**
