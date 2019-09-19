## Lab-4
This is a guide to Lab 4 in HCS 7194. In this lab we will practice the inspection of files. We will use the commands ```ls```, ```wc```, ```head```, ```tail```, and the use of piping (|), as well as the use of ```less```, ```grep```, and ```gunzip```.

## Preliminaries
For this Lab you should have in your computer already installed the following software
* GitHub Desktop: https://desktop.github.com
* Atom: https://flight-manual.atom.io/getting-started/sections/installing-atom/ (optional, since it is not fully functional in Windows OS).
  * In Atom, go to the menu Preferences, Install and type: Atom IDE Terminal by Qicrosoft, install the package and restart Atom
# It does not work in Windows

## Let's start simple: connect to OSC using ssh and your terminal
```
ssh username@owens.osc.edu
# Provide your OSC password and you should be able to see the messages and prompt from OSC
# Windows users will need to use a tool such as MobaXterm or PuTTY
```
## Preparing the directory structure and get the first file
Then, let's go to our working directory and make a new directory called ```Lab_5```
```
# Checking in which directory you are located
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
A file called sonnets.txt should be in your directory, let's start to inspect it:
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
# to go to the next result type n
# to go to the previous result type N
# if you want to go to the end of the file use G
# if you want to go to the beginning of the file use 1G
```
What does happen? Use the key Q to quit the file
Look for the line "Let me not" in the file.

Now, let's use the command ```wc```
```
man wc
wc --help
wc sonnets.txt
# the output indicates how many lines, words and bytes thera are in the file
```

Now let's use grep:
```
# Looking for the occurrences of the string "All"
grep All sonnets.txt
# Piping using grep and wc
grep rose sonnets.txt | wc
# grep is case sensitive, then try
grep -i rose sonnets.txt | wc
# using a regular expression:
greep 'ro[a-z]*s' sonnets.txt
# what is happening in the following cases?
grep -c All sonnets.txt
grep -n -C 2 All sonnets.txt
grep -m2 All sonnets.txt
grep -n rose sonnets.txt
```

Let's use head and append:
```
head sonnets.txt > sonnets_head.txt
wc sonnets_head.txt
```
What if we use pipes?
```
head sonnets.txt | wc
```
How the command would look like using tail?
Let's try to extract the first sonnet?
```
head -n 18 sonnets.txt | tail -n 14
```

## Manipulating Fastq and Fasta Files

We will practice manipulating some fastq and fasta files. First we will look at a file produced from a paried-end RNA Sequencing experiment. To do this, we will download the data from a sequence archive and extract the fastq files using SRA Toolkit.

Make sure you are in the Lab_5 directory and load the SRA toolkit into your environment on the super computer
```
wget https://ftp-trace.ncbi.nlm.nih.gov/sra/sdk/current/sratoolkit.current-ubuntu64.tar.gz -O sratoolkit.tar.gz

tar xvzf sratoolkit.tar.gz

rm sratoolkit.tar.gz

./sratoolkit.2.9.6-1-ubuntu64/bin/fasterq-dump

```

Make a new directory to store your fastq files from an RNASeq experiment
```
mkdir RNASeq_Data
```
Retrieve a file from the NCBI Sequence Read Archive
```
wget https://sra-downloadb.be-md.ncbi.nlm.nih.gov/sos/sra-pub-run-2/SRR1649142/SRR1649142.1 -O RNASeq_Data/SRR1649142.sra
```
Extract fastq files from the sra file using sratoolkit
```
./sratoolkit.2.9.6-1-ubuntu64/bin/fasterq-dump RNASeq_Data/SRR1649142.sra -O RNASeq_Data/ -p
```
View fastq file with less
```
less RNASeq_Data/SRR1649142.sra_1.fastq

less RNASeq_Data/SRR1649142.sra_2.fastq
```
Count the number of lines in each fastq file to see if they match. The files represent the forward and reverse reads from a paired end RNA sequencing experiment.
Do you expect them to match?
```
cat RNASeq_Data/SRR1649142.sra_1.fastq | wc -l

cat RNASeq_Data/SRR1649142.sra_2.fastq | wc -l
```
How would you get the number of reads in a fastq file?
```
cat RNASeq_Data/SRR1649142.sra_1.fastq | echo $((`wc -l`/4))

cat RNASeq_Data/SRR1649142.sra_2.fastq | echo $((`wc -l`/4))
```

Make a directory to store your genome file and gff file
```
mkdir Genome_Files

cd Genome_Files
```
Download the fasta file and gff file for the peach (Prunus persica) genome from the following site: https://www.rosaceae.org/species/prunus_persica/genome_v2.0.a1
```
wget ftp://ftp.bioinfo.wsu.edu/www.rosaceae.org/Prunus_persica/Prunus_persica-genome.v2.0.a1/assembly/Prunus_persica_v2.0.a1_scaffolds.fasta.gz

wget ftp://ftp.bioinfo.wsu.edu/www.rosaceae.org/Prunus_persica/Prunus_persica-genome.v2.0.a1/assembly/Prunus_persica_v2.0.a1_scaffolds.gff3.gz

zless Prunus_persica_v2.0.a1_scaffolds.fasta.gz

zless Prunus_persica_v2.0.a1_scaffolds.gff3.gz
```
Extract only chromosome 2 sequences from our fasta file
```
gunzip -c Prunus_persica_v2.0.a1_scaffolds.fasta.gz > Prunus_persica_v2.0.a1_scaffolds.fasta

awk 'BEGIN {RS=">"} /Pp02/ {print ">"$0}' Prunus_persica_v2.0.a1_scaffolds.fasta > Chromosome2.txt

head Chromosome2.txt

```
