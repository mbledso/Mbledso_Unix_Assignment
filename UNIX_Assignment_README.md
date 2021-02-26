# <h1>Unix Assignment
___
## Data Inspection

### Attributes of fang\_et\_al_genotypes

The following commands were used to inspect this file:

Inspecting file size:
>du -h fang\_et\_al_genotypes.txt

Inspecting number of columns:
>tail -n 1 fang\_et\_al_genotypes.txt |awk -F "\t" '{print NF; exit}'

Inspecting number of lines, words, and characters:
>wc fang\_et\_al_genotypes.txt

Inspecting ASCII text:
>file fang\_et\_al_genotypes.txt

By inspecting this file I learned that:

1. The file size is 6.1M
2. There are 986 columns in total 
3. There are 2783 lines in total 
4. There are 2744038 words in total
5. There are 11051939 characters in total
6. ASCII text is present and are very long lines


### Attributes of snp_position

The following commands were used to inspect this file:

Inspecting file size:
>du -h snp_position.txt

Inspecting number of columns:
>tail -n 1 snp_position.txt |awk -F "\t" '{print NF; exit}'

Inspecting number of lines, words, and characters: 
>wc snp_position.txt

Inspecting ACSII text:

>file snp_position.txt

By inspecting this file I learned that:

1. The file size is 38K
2. There are 15 columns in total 
3. There are 984 lines in total 
4. There are 13198 words in total 
5. There are 82763 characters in total 
6. ASCII text is present

___
## Data Processing

### Maize Data
Sorting and looking at the file:
>grep -v "^#" fang\_et\_al_genotypes.txt | cut -f 3| sort | uniq -c

Removing ZMMIL, ZMMLR, and ZMMMR groups and adding them into a new file:
> grep "ZMM" fang\_et\_al_genotypes.txt > genotype_maize.txt

Removing the headers from fang\_et\_al_genotypes.txt and adding them into a new file:
> head -n 1 fang\_et\_al_genotypes.txt > header.txt

Combining the header.txt and genotype_maize.txt files:
> cat header.txt genotype\_maize.txt > header\_genotype\_maize.txt

Transposing my header\_genotype\_maize.txt file:
>awk -F transpose.awk header\_genotype\_maize.txt > transposed\_genotype\_maize.txt

Sorting my transposed file:
>tail -n +4 transposed\_genotype\_maize.txt > sorted\_transposed\_genotype\_maize.txt

Check if headers were sorted correctly:
>cut -f 1 sorted\_transposed\_genotype\_maize.txt

Checking if the snp_position.txt file is sorted:
>sort -c snp_position.txt

Removing headers in snp_position.txt before sorting it:
>head n -1 snp_postion.txt > snp_headers.txt

Sort snp_position.txt file and checking that is was sorted correctly:
>tail -n +2 snp_position.txt | sort -k1,1 > sorted\_snp\_position.txt
> cut -f 1 sorted\_snp\_position.txt


Join the sorted snp file and the transposed genotype file:
>join -1 1 -2 1 sorted\_snp\_position.txt sorted\_transposed\_genotype\_maize.txt > joined\_snp\_transposed\_genotype\_maize.txt

Checking the number of lines in all files to see if they joined correctly:
>wc -l sorted\_snp\_position.txt sorted\_transposed\_gentoype\_maize.txt joined\_snp\_transposed\_genotype\_maize.txt

>
>986 986 986 
> 

Sorting the new joined file by chromosome number (column 3):
>sort -k3 joined\_snp\_transposed\_genotype\_maize.txt | head

Separating the joined file into separate files for each chromosome:
>cat joined\_snp\_transposed\_genotype\_maize.txt | awk '$3 == "1" {print;}' > Chromosome\_1_\maize.txt

\#Above shows the command for organizing all of the data for chromosome 1 into one file, but this was repeated with every chromosome (1-10)

Sorting my Chromosome files based on decreasing position values:
>sort -k4,4 -r Chromosome\_1\_maize.txt | awk '$4 != "multiple" && $4 != "unknown" {print;}' > decreasing\_sorted\_only\_Chromosome\_1\_maize.txt

\#To remove the "unknown" and "multiple" positions in our Chromosome files I piped the sorted file to an awk command that pulls everything except those positions into a new file.

Replacing missing data encoded with "-" in my Decreasing Chromosome files:
>sed 's/?/-/g' decreasing\_sorted\_only\_Chromosome\_1\_maize.txt > completed\_decreasing\_sorted\_Chromosome\_1\_maize.txt

Sorting my Chromosome files based on increasing position values:
> sort -k4,4 Chromosome_1_maize.txt | awk '$4 != "multiple" && $4 != "unknown" {print;}' > completed\_increasing\_sorted\_Chromosome\_1\_maize.txt

\# in the increasing files, the missing data encoded is already labeled as "?"

Removing SNPs with unknown positions into its own file:
>cat joined\_snp\_transposed\_genotype\_maize.txt | awk '$4 == "unknown" {print;}' > SNPs_unknown_positions_maize.txt

\# Now all SNPs in unknown positions are in one file together

Removing SNPs with multiple positions into its own file:

>cat joined\_snp\_transposed\_genotype\_maize.txt | awk '$4 == "multiple" {print;}' > SNPs_multiple_positions_maize.txt

\# Now all SNPs in multiple positions are in 1 file together 

\# All files for maize are completed

Adding the completed files onto my git repository:
>mv completed* /home/mbledso/Mbledso_Unix_Assignment/Maize_completed_files

>mv SNPs_unknown_positions.txt /home/mbledso/Mbledso_Unix_Assignment/Maize_completed_files

> mv SNPs_multiple_positions.txt /home/mbledso/Mbledso_Unix_Assignment/Maize_completed_files

\#I can now add, commit, and push these files onto my remote repository
>git add .
>git commit -m "adding my completed maize files"
>git push

\#I have completed the Data Assessment for Maize

### Teosinte

Sorting and looking at the file:
>grep -v "^#" fang\_et\_al_genotypes.txt | cut -f 3| sort | uniq -c

Removing ZMMIL, ZMMLR, and ZMMMR groups and adding them into a new file:
> grep "ZMP" fang\_et\_al_genotypes.txt > genotype_teosinte.txt

Removing the headers from fang\_et\_al_genotypes.txt and adding them into a new file:
> head -n 1 fang\_et\_al_genotypes.txt > header_teosinte.txt

Combining the header.txt and genotype_teosinte.txt files:
> cat header.txt genotype\_teosinte.txt > header\_genotype\_teosinte.txt

Transposing my header\_genotype\_teosinte.txt file:
>awk -F transpose.awk header\_genotype\_teosinte.txt > transposed\_genotype\_teosinte.txt

Sorting my transposed file:
>tail -n +4 transposed\_genotype\_teosinte.txt > sorted\_transposed\_genotype\_teosinte.txt

Check if headers were sorted correctly:
>cut -f 1 sorted\_transposed\_genotype\_teosinte.txt

Checking if the snp_position.txt file is sorted:
>sort -c snp_position.txt

\#I already have the headers from the snp_position.txt file in snp_headers.txt

Check to see if the sorted\_snp\_position.txt:
> cut -f 1 sorted\_snp\_position.txt 

Join the sorted snp file and the transposed genotype file:
>join -1 1 -2 1 sorted\_snp\_position.txt sorted\_transposed\_genotype\_teosinte.txt > joined\_snp\_transposed\_genotype\_teosinte.txt

Checking the number of lines in all files to see if they joined correctly:
>wc -l sorted\_snp\_position.txt sorted\_transposed\_gentoype\_teosinte.txt joined\_snp\_transposed\_genotype\_teosinte.txt

>
>986 986 986 
> 

Separating the joined file into separate files for each chromosome:
>cat joined\_snp\_transposed\_genotype\_teosinte.txt | awk '$3 == "1" {print;}' > Chromosome\_1_\teosinte.txt

\#Above shows the command for organizing all of the data for chromosome 1 into one file, but this was repeated with every chromosome (1-10)

Sorting my Chromosome files based on decreasing position values:
>sort -k4,4 Chromosome\_1\_maize.txt | awk '$4 != "multiple" && $4 != "unknown" {print;}' > decreasing\_sorted\_Chromosome\_1\_teosinte.txt

\#To remove the "unknown" and "multiple" positions in our Chromosome files I piped the sorted file to an awk command that pulls everything except those positions into a new file.

Replacing missing data encoded with "-" in my Decreasing Chromosome files:
>sed 's/?/-/g' decreasing\_sorted\_only\_Chromosome\_1\_teosinte.txt > completed\_decreasing\_sorted\_Chromosome\_1\_teosinte.txt

Sorting my Chromosome files based on increasing position values:
> sort -k4,4 Chromosome_1_teosinte.txt | awk '$4 != "multiple" && $4 != "unknown" {print;}' > completed\_increasing\_sorted\_Chromosome\_1\_teosinte.txt

\# in the increasing files, the missing data encoded is already labeled as "?"

Removing SNPs with unknown positions into its own file:
>cat joined\_snp\_transposed\_genotype\_teosinte.txt | awk '$4 == "unknown" {print;}' > SNPs\_unknown\_positions\_teosinte.txt

\# Now all SNPs in unknown positions are in one file together

Removing SNPs with multiple positions into its own file:

>cat joined\_snp\_transposed\_genotype\_maize.txt | awk '$4 == "multiple" {print;}' > SNPs\_multiple\_positions\_maize.txt

\# Now all SNPs in multiple positions are in one file together 

\# All files for maize are completed

Adding the completed files onto my git repository:
>mv completed* /home/mbledso/Mbledso\_Unix\_Assignment/Teosinte\_completed\_files

>mv SNPs_unknown_positions.txt /home/mbledso/Mbledso\_Unix\_Assignment/Teosinte\_completed\_files

> mv SNPs_multiple_positions.txt /home/mbledso/Mbledso\_Unix\_Assignment/Teosinte\_completed\_files

\#I can now add, commit, and push these files onto my remote repository
>git add .
>git commit -m "adding my completed maize files"
>git push

\#I have completed the Data Assessment for Teosinte
 




