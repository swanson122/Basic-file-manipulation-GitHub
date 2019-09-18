## Lab-5
This is a guide to Lab 5 in HCS 7194. In this lab we will practice the inspection of files. We will use the commands ```ls```, ```wc```, ```head```, ```tail```, and the use of piping (|), as well as the use of ```less```, ```grep```, and practice with ```tar``` and ```gunzip```.

## Preliminaries
For this Lab you should have in your computer already installed the following software
* GitHub Desktop: https://desktop.github.com
* Atom: https://flight-manual.atom.io/getting-started/sections/installing-atom/
  * In Atom, go to the menu Preferences, Install and type: Atom IDE Terminal by Qicrosoft, install the package and restart Atom

## Let's start simple: connect to OSC using ssh and your terminal
```
ssh username@owens.osc.edu
# Provide your OSC password and you should be able to see the messages and prompt from OSC
```
## Preparing the directory structure and get the first file
Then, let's go to our working directory and make a new directory called ```Lab_5```
```
# Cheking in which directory you are located
pwd
# Moving to the correct working directory
cd /fs/scratch/PAS####/username
# Creating the new directory
mkdir Lab_5
# Listing your directories
ls
# Entering into the directory Lab_5
cd Lab_5
```
Now, let's get a basic text file by using wget:
```
wget cdn.learnenough.com/sonnets.txt
```
A file called sonnets.txt should be in your directory, let's start to inspect it.
```
ls -l sonnets.txt
ls -lh sonnets.txt
tail sonnets.txt
head -n 18 sonnets.txt
less sonnets.txt
```
You can scroll using your keyboard. Do you have an idea what is this file?
Now type:
```
/rose
```
What does happen? Use the key Q to quit the file
Now let's use grep:
```
grep All sonnets.txt
grep -i All sonnets.txt
grep -c All sonnets.txt
grep -n -C 2 All sonnets.txt
grep -m2 All sonnets.txt
grep -n rose sonnets.txt
```
Now, let's use the command ```wc```
```
man wc
wc --help
wc sonnets.txt
```
#Load the SRA toolkit into your environment on the super computer

$module spider SRA toolkit

$module load sratoolkit/2.9.0

#Read about the options of the command fastq-dump

$fastq-dump -h

#Make a new directory to store your fastq files from an RNASeq experiment

$mkdir RNASeq_Files

#Retrieve a file from the NCBI Sequence Read Archive

$fastq-dump SRR1649142 -O RNASeq_Files --split-files --gzip

#While waiting for the files to download, you can spend time exploring the NCBI SRA

#View fastq file with zless (zless works the same as less however it works on uncompressed files)

$zless /RNASeq_Data/SRR1649142_1.fastq.gz

$zless /RNASeq_Data/SRR1649142_2.fastq.gz

#Count the number of bytes in each fastq file to see if they match. They represent the forward and reverse reads from a paired end RNA sequencing experiment
#Do you expect them to match?

$zcat /RNASeq_Data/SRR1649142_1.fastq.gz | wc -l

$zcat /RNASeq_Data/SRR1649142_2.fastq.gz | wc -l

#How would you get the number of reads in a fastq file?

$zcat /RNASeq_Data/SRR1649142_1.fastq.gz | echo $(('wc -l'/4))

$zcat /RNASeq_Data/SRR1649142_2.fastq.gz | echo $(('wc -l'/4))

#Unzip your fastq files using gunzip, however be sure to include the option -c; this will ensure you save an unmodified compressed version of your raw data

$gunzip -c /RNASeq_Data/SRR1649142_1.fastq.gz > /RNASeq_Data/SRR1649142_1.fastq

$gunzip -c /RNASeq_Data/SRR1649142_2.fastq.gz > /RNASeq_Data/SRR1649142_2.fastq

#Make a directory to store your genome file and gff file

$mkdir Genome_Files

$cd Genome_Files

#Download the fasta file and gff file for the peach (Prunus persica) genome from the following site: https://www.rosaceae.org/species/prunus_persica/genome_v2.0.a1

$wget ftp://ftp.bioinfo.wsu.edu/www.rosaceae.org/Prunus_persica/Prunus_persica-genome.v2.0.a1/assembly/Prunus_persica_v2.0.a1_scaffolds.fasta.gz

$wget ftp://ftp.bioinfo.wsu.edu/www.rosaceae.org/Prunus_persica/Prunus_persica-genome.v2.0.a1/assembly/Prunus_persica_v2.0.a1_scaffolds.gff3.gz

$zless Prunus_persica_v2.0.a1_scaffolds.fasta.gz

$zless Prunus_persica_v2.0.a1_scaffolds.gff3.gz

#Extract only chromosome 2 sequences from our fasta file

$gunzip -c Prunus_persica_v2.0.a1_scaffolds.fasta.gz > Prunus_persica_v2.0.a1_scaffolds.fasta

$awk 'BEGIN {RS=">"} /Pp02/ {print ">"$0}' Prunus_persica_v2.0.a1_scaffolds.fasta > Chromosome2.txt #Or you could call it Chromosome2.fasta
