# `03-DataAnalysis.md` 

#Rerunning the analysis described in the manuscript using Bowtie2 and TSAS.

This markdown file goes through the pipeline that our group used to analyze the processed data from ```02-DataProcessing.md```, which were mainly described in the manuscript. The TSAS and Bowtie2 user guides/manuals can be found here: 

[https://github.com/srimam/TSAS]()

[http://bowtie-bio.sourceforge.net/bowtie2/manual.shtml]()



###This Markdown Contains Three Essential Steps:

1- Installations and downloads

2- Mapping Tn-Seq Reads to Reference Genome using ```bowtie2```

3- Using Tn-Seq Analysis Software (TSAS) for Met-Seq data manipulation and statistical analyses

##1) Installations

1. Install Java -

	* For MacOS with Intel processors: 
		
		Install Java here : [https://www.oracle.com/java/technologies/javase-jdk15-downloads.html](https://www.oracle.com/java/technologies/javase-jdk15-downloads.html)
	* For MacOS with M1 chip processors, run the following codes: 	 
		
		```
		$ curl -s "https://get.sdkman.io​" | bash  #install SDK
	
		$ sdk list java #lists the java versions
	
		$ sdk install java 8.0.282-zulu 
		
		$ java -version
		```
		
2. Git clone the TSAS git hub repo: [https://github.com/srimam/TSAS](https://github.com/srimam/TSAS)
NOTE: Keep these files in the same folder you have your processed genome fasta and TnSeq data files. Use the latest version for these analyses. 

3.  Download the Reference Annotation GFF file (NCBI) 

	* To download the GFF file: 
		* Follow the link to "Assembly" under "Related information" in the right-hand sidebar
		* Click on the "Download Assembliy" button to open the download menu
		* Select the "Source database" - GenBank 
		* Download the necessary file
	
## 2) Map Tn-Seq reads to Indexed Reference Genome 
1. Navigate to the folder for which you have saved your processed files after following steps from ```02-DataProcessing.md```. 
	
1. Run ```bowtie2``` to map your trimmed TnSeq reads to the indexed genome file : 

	Format: ```bowtie2 [options] -x indexFile fastqFile -S outputFile```
	
	```
	bowtie2 -x MPAO_index SRR13258538trimmed.fq -S 538_mapped.bowtie2
	```
	
	**NOTE: The output in your terminal should tell you the % that mapped to the reference genome index. This information is not saved anywhere else. It may be worthwhile to copy and paste this information in a text edit application.** 
	
	Keep the output file named ```538_mapped.bowtie2``` from this command - you will need this to run TSAS. 
	
## 3) Run Tn-Seq Analysis Software (TSAS) V.3.1 on mapped reads for manipulation and statistical analyses
 
1. Fill the "Parameters.txt" file located in the TSAS folder you cloned locally to your computer. Keep most at default parameters. The manuscript ran a one sample analysis and changed the threshold of the number of hits to unique insertion site to **10.** We also changed the "results" from default "short" to "long." 
	
	```
	#####Input parameters example: 
	
	##Analysis type (1 - one sample analysis (default); 2 - two sample analysis)
	Analysis_type = 1
	
	##Genome FASTA file [required]
	Genome_sequence = MPAO1_genome.fasta
	
	##Organism GFF file [required]
	GFF = MPAO1_genome.gff
	
	##Mapped reads file format (Bowtie, SOAP or Eland) [required]
	Mapped_format = Bowtie
	
	##Mapped reads files (replicates should be listed sequentially in appropriate group (control or treatment) separated by commas)
	
	#Treatment [required] (if replicate samples are provided, analysis will be averaged over the replicates)
	Treatment = 538_mapped.bowtie2
	
	#Control (this is only used with two sample analysis and will be ignored by one-sample analysis code. For a one-sample, sample(s) should be list under Treatment. If replicate samples are provided, analysis will average over the replicates)
	Control = 
	
	##Threshold for minimum number of hits at a unique insertion site for it to be considered a true site. Default value is 5 hits.
	Min. hits = 10
	
	##Percentage of start and end of gene to be discarded when determining essentiality (a value of 5 equals 5% to be ignored @ start and @ end by default)
	Clipping = 5
	
	##Capping of reads at unique sites to minimize PCR/sequencing bias (0 - no capping; or 1 - capping with average hits per insertion site + 2 st. dev. (default); or 2 - capping with average hits per insertion site; or 3 - capping with median hits per insertion site) - only relevant to 2 sample analysis
	Capping = 1
	
	##Weight reads per gene based on number of unique insertions in a gene (total hits*(unique hits per gene/average unique hits per gene)) 0 - No weights 1 - Weights used (- only relevant to 2 sample analysis)
	Weights = 0
	
	##Transposon sequence specificity (i.e., target sequence of transpson e.g., TA for Mariner transpons). Leave blank from random transposons like Tn5. This is only relevant for one sample analysis.
	Target seq = TA
	
	##Result format (Long or Short)(if Long it provides additional columns for capped and weighted reads, as well as unadjusted pvalues)
	Result = long	
	
	```

2. Run the following code to run TSAS:
	
	```
	$ java TSAS
	
	```
**NOTE: After running this command, the output in your terminal will tell you the number of total number of unique hits, total number of unique hits within genes, and total number of reads within genes. This information is not generated anywhere else, so make sure you copy and paste this into a text file and save it.**

9. During the course of a TSAS analysis, two or more result files are produced. These files include:
	
	* a. WIG files: For each aligned/mapped reads file specified, a WIG format file is produced containing the number of mapped reads per base pair for the entire genome. This file can be loaded into a genome browser, such as MochiView, to visualize the distribution of insertion sites (and number reads at each insertion site) across the genome. The WIG files are named after the aligned reads file from which the data was derived. The WIG files will be located in the same directory as the aligned reads files.
	
	* b. Essential_genes.txt (one-sample analysis only): This file can be opened in excel and contains a list of all annotated genes in the genome along with all the statistics calculated for each gene from the data provided for a one-sample analysis. This file will be located in the directory from which TSAS is called. This file has the following columns:
		
		1. **Gene ID:** the unique ID for each gene obtained from the provided GFF file.
		2. **Annotation:** the annotation for each gene obtained from the provided GFF file.
		3. **Gene length (bp):** the length of each gene in base pairs.
		4. **No. of Unique hits:** the number of unique insertion sites for each gene.
		5. **Normalize Unique hits (hits/bp):** the number of unique insertion sites for each gene normalized to the length of the gene.
		6. **Total number of reads:** total number of reads for each gene.
		7. **Pvalue (Essential):** The unadjusted p-values calculated from a binomial distribution
		assessing the likelihood of essentiality of each gene. (Omitted from short results
		output).
		8. **Adj. Pvalue (Essential):** p-values from column 7 corrected for multiple testing using
		the Benjamini-Hochberg (BH) method.
		9. **FWER (Essential):** the family wide error rate calculated from p-values in column 7
		(Bonferroni correction).
		10. **Pvalue (Improved fitness):** The unadjusted p-values calculated from a binomial
		distribution assessing the likelihood of genes whose disruption by transposon insertions may result in an overall improvement of fitness. (Omitted from short results output).
		11. **Adj. Pvalue (Improved fitness):** p-values from column 10 corrected for multiple testing using the BH method.
		12. **FWER (Improved fitness):** the family wide error rate calculated from p-values in column 10 (Bonferroni correction).


	
