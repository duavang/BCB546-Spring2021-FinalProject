# `04-ProcessingPart2.md`

This file describes how the data were processed to format them for plotting with ggplot in R.

Updated by Marissa Stroud on April 29, 2021.

**Marissa needs to make this file more concise before the project is turned in**

## Figure 2E or F?

**Add information in if there was processing of the data before it was plotted.**

## Figure 3A

### Background

The TSAS data is a little difficult to manipulate and plot in the format it is in, as there is no information about gene position on the chromosome, just gene number. Because of this, we needed to download the annotation feature table for MPAO1 that contains important information about the genes (start/end sites, +/- strand) and merge that information into the TSAS information. Additionally, the TSAS information was stored in 4 individual files, and analyzing a single data table would make the analysis much simpler.  

### MPAO1 Feature Table Manipulation

The `MPAO1_feature_table.txt` file had multiple types of information within it, containing both gene and coding sequence data. These lines had nearly identical information, so it wasn't worthwhile to keep both of them. There are a few more genes than CDS due to the fact that tRNA and rRNA genes are also included. Thus, I decided to grep out the "gene" information only to prevent redundancy of information.

	grep gene MPAO1_feature_table.txt | wc -l
		5955
	grep CDS MPAO1_feature_table.txt | wc -l
		5847
   
	grep "^#" MPAO1_feature_table.txt > MPAO1_feature_table_genes.txt 
	grep gene MPAO1_feature_table.txt >> MPAO1_feature_table_genes.txt

The file `MPAO1_feature_table_genes.txt` is the one used for the rest of the analysis. 

I used this command to grab just the 4 columns of data we'd actually want to use. This contains the following columns: `start` and `end` position data for each gene (columns 8 and 9), and `strand` (+/-) data for which strand of the DNA the gene lies on (column 10), and the `Gene_ID` (column 17). 

```
awk `{ print $8 "\t" $9 "\t" $10 "\t" $17}` MPAO1_feature_table_genes.txt > 
	MPAO1_features_abbrev.txt
```

### TSAS file manipulation

Before merging the MPAO1 file with the 4 Essential gene files, I realized some of them weren't in ASCII format, which was causing problems with some of my commands. This might have been because they were downloaded from an email. The dos2unix program (installed with `Homebrew`) fixed this issue for me. 

	brew install dos2unix
	
	dos2unix MPAO1_feature_table_abbrev.txt Ess538.txt Ess539.txt 
		Ess540.txt Ess541.txt			# converting to ASCII format

I renamed the files to something shorter so they'd be easier to work with once the names are appended to the end of each row. I also removed the `.txt` after each file name so that wouldn't be appended alongside the file name. The 3-digit number corresponds to the last 3 digits in the read files downloaded from SRA (ex. Ess**540** corresponds to the original file SRR13258**540**.fastq).

	mv Essential_genes-538.txt Ess538		#Renaming files
	mv Essential_genes-539.txt Ess539		#Renaming files
	mv Essential_genes-540.txt Ess540		#Renaming files
	mv Essential_genes-541.txt Ess541		#Renaming files

The next set of commands adds the file name to the last column of each of the files and concatenates all 4 into a single file. Because this command added all 4 headers to the file, I used `head` to pull out the first line, then renamed the new last row to "Library". After that, I used `grep` to pull out all the rows containing "MPAO1", which is included in every row within the name of each gene. 

```
for file in Ess5*; do awk 'BEGIN{OFS="\t"}{print $0, FILENAME}' 
	$file; done > EssGenesAll.txt
	
head -n 1 EssGenesAll.txt > EssGenesAll_clean.txt
sed "s/Ess538/Library/" EssGenesAll_clean.txt > EssGenesAll_clean2.txt
grep MPAO1 EssGenesAll.txt | sort -k1,1 >> EssGenesAll_clean2.txt 
```

### Merging the files

TSAS code doesn't include position information about the genes for plotting purposes, so I merged this file with the feature table. This code joined the two files, with the output being a tab-delimited text file. This file is now ready for analysis in R. 

```
join -1 1 -2 1 -t "$(printf '\t')" MPAO1_feature_table_abbrev.txt 
	EssGenesAll_clean2.txt > EssGenesAll_Merge.txt
	
```

This file will be imported into R for plotting with ggplot in the file `05-ggplotFigures.Rmd`.

## Figure 3B or 3C?

**Add information in if there was processing of the data before it was plotted.**
