# `01-InitialDataStructure.md`

Downloading, inspecting, and describing the data utilized in the study. Also a quick overview of the packages and tools used to perform this study. 

## Software & Packages used in this study

* SRA Toolkit version 2.11.0, AKA sra-tools ([LINK](https://github.com/ncbi/sra-tools))
* trim_galore ([LINK](https://www.bioinformatics.babraham.ac.uk/projects/trim_galore/))
* bowtie2 ([LINK](https://github.com/BenLangmead/bowtie2.git))
* Tn-Seq Analysis Software (TSAS) V0.3.0 ([LINK](https://github.com/srimam/TSAS.git))
* R version 4.0.4 (2021-02-15) -- "Lost Library Book"



## Sequence Read Archive (SRA) files

### Downloading files

I downloaded 4 files from the Sequence Read Archive on April 14, 2021: `SRR13258538`, `SRR13258539`, `SRR13258540`, `SRR13258541`. Using the `sra-toolkit` package downloaded from [NCBI on GitHub](https://github.com/ncbi/sra-tools/wiki), the files were converted into FASTQ format.      



### General information about the files:

**File Name, Experiment Accession, Sample Accession, Total Bases, and Library Name**

	File name		ExperimentAcc	SampleAcc	Length		Name	
	SRR13258538 	SRX9689339 		SRS7886629 	41705709bp 	E3_enrichment
	SRR13258539 	SRX9689338 		SRS7886628 	48172356bp 	E2_enrichment
	SRR13258540 	SRX9689337 		SRS7886627 	43481631bp 	E1_enrichment
	SRR13258541 	SRX9689336		SRS7886626 	34329120bp 	Library

### File dimensions

	wc SRR13258538.fastq SRR13258539.fastq SRR13258540.fastq SRR13258541.fastq

	Lines		Words		Characters		File name
	3271036 	6542072 	145116682 		SRR13258538.fastq
	3778224 	7556448 	167686548 		SRR13258539.fastq
	3410324 	6820648 	151314998 		SRR13258540.fastq
	2692480 	5384960 	119370940 		SRR13258541.fastq

### File size

	du -h SRR13258538.fastq SRR13258539.fastq SRR13258540.fastq SRR13258541.fastq
			
	144M	SRR13258538.fastq
	161M	SRR13258539.fastq
	160M	SRR13258540.fastq
	128M	SRR13258541.fastq

### Number of rows of data 
There are 4 rows of data per entry. Each entry starts with @FILENAME, so counting this for each file gives an accurate number of entries. This is equal to the number of lines divided by four.

	grep "^@SRR13258538" SRR13258538.fastq | wc -l		817759
	grep "^@SRR13258539" SRR13258539.fastq | wc -l 		944556
	grep "^@SRR13258540" SRR13258540.fastq | wc -l 		852581
	grep "^@SRR13258541" SRR13258541.fastq | wc -l 		673120
	
	
## *Pseudomonas aeruginosa* MPAO1 genome & annoation files

### Downloading files

The *Pseudomonas aeruginosa* MPAO1 genome (FASTA format) and annotation files (both CSV and GFF formats) were downloaded from NCBI under the GenBank accession number: [CP027857.1](https://www.ncbi.nlm.nih.gov/nuccore/CP027857.1?report=genbank). 

### File dimensions

`MPAO1_genome.fasta`

* This genome is 6275467 bp in length according to the NCBI website. 

`MPAO1_feature_table.txt`

* By using `wc` on this document, we can see that it has 11853 lines, 174264 words, and 1642978 characters. Of this information, only the number of lines is really relevant, as there is one line per gene or CDS.
	* `grep "^gene" MPAO1_feature_table.txt | wc -l` returns the number of lines corresponding to genes in this genome. This is equal to **5926** genes. 
	* `grep "^CDS" MPAO1_feature_table.txt | wc -l` returns the number of coding sequences (CDS) corresponding to proteins produced by this genome. This is equal to **5847** proteins. 
	* The number of genes and proteins are not equal, as "genes" also include tRNA/rRNA genes that are not translated into protein. 

`MPAO1_annotation.gff`

	tail MPAO1_annotation.gff | awk -F "\t" '{print NF; exit}'
	9
	wc MPAO1_annotation.gff 
	5958   75014  789755 
	
* This file has 9 columns, 5958 lines, 75014 words, and 789755 characters. 

### File size

	du -h MPAO1_genome.fasta MPAO1_feature_table.txt MPAO1_annotation.gff
	
		7.0M	MPAO1_genome.fasta
		1.6M	MPAO1_feature_table.txt
		772K	MPAO1_annotation.gff


## Supplementary material data tables

### Downloading files

The supplementary data tables from this paper were downloaded from the journal's website [here](https://msystems.asm.org/content/6/1/e00933-20). 

### File dimensions

`TableS1A.csv`: This was the only table used in our analysis.

* Using `wc`, we see that this file has 224 lines, 871 words, and 19333 characters. 
* `tail TableS1A.csv | awk -F "," '{print NF; exit}'` tells us that this file has 14 columns. 

### File size

	du -h TableS1A.csv 
		20K		TableS1A.csv
