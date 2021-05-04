# `02-DataProcessing.md`

This markdown explains the pipeline in which we used to process the reference genome fasta file and Met-Seq data and format them for the analysis in ```03-DataAnalysis.md```, which were described in the manuscript. The commands and userguides used in this markdown (```bowtie2``` and ```Trimgalore```) can be found here:

[http://bowtie-bio.sourceforge.net/bowtie2/manual.shtml]()

[https://github.com/FelixKrueger/TrimGalore/blob/master/Docs/Trim_Galore_User_Guide.md](https://github.com/FelixKrueger/TrimGalore/blob/master/Docs/Trim_Galore_User_Guide.md)

### This markdown includes 4 essential parts: 

1- Downloading Genome Files

2- Command line Installations 

3- Reference Genome Processing 

4- Trimming Met-Seq Files for Data Analysis


##1) Downloading Genome Files:
1. Download the Reference Genome FASTA file (can be downloaded from NCBI) 
*([MPAO1 GenBank: CP027857.1](https://www.ncbi.nlm.nih.gov/nuccore/CP027857.1?report=genbank))
	* To download the FASTA file: 
		* Click on FASTA.   
		* Click on "Send to:"
		* Choose destination to "File"
		* Keep format as "FASTA"
		* Click "create file"


2. Download the Met-Seq Data Files (unzipped).

## 2) Command line Installations

3. Install ```bowtie2``` from conda: 

	```
	$ conda install bowtie2
	```
	
4. Install ```trimgalore```: [https://github.com/FelixKrueger/TrimGalore]() 

	```
	$ git clone https://github.com/FelixKrueger/TrimGalore``
	
	$ export $PATH
	```

	
## 3) Reference Genome Index Processing 

1. Use ```bowtie2``` to index your reference genome: 
 
 Format: ```bowtie2-build <genome fasta file> <output name>```
	
	```
	$ bowtie2-build MPAO1_genome.fasta MPAO_index
	```
	
	This command will compute out several files with the same name in different formats. That is okay - the next few commands will only be using the "base" name of those files. 
	
2. Inspect the index file by running: 
	
	Format: ```bowtie2-inspect [options] <ebwt_base>```

	```
	$ bowtie2-inspect MPAO_index
	```
	**Note:  Only the "basename" of the index is to be inspected. The basename is name of any of the index files but with the .X.ebwt or .rev.X.ebwt suffix omitted. bowtie-inspect first looks in the current directory for the index files, then in the directory specified in the BOWTIE_INDEXES environment variable.**
	
## 4) Trimming Met-Seq files

3. Trim your reads using ```trimgalore```: 

	```
	$ trim_galore --length 20 --phred20 SRR13258538.fastq
	
	```
	
	Your output fill will be a SRR13258538trimmed.fastq file. Save this file in same folder as your indexed genome.




####**Next step --> follow ```03-DataAnalysis.md``` for data analysis.**
