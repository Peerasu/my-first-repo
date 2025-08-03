# Massively Parallel Burrows-Wheeler Alignment for Microbiome Analysis over Plant Genomic Data

# Plant Microbiome Analysis Pipeline
This repository contain the pipeline presented in the paper "Massively Parallel Burrows-Wheeler Alignment for Microbiome Analysis over Plant Genomic Data". In the paper we mainly talk about step 2, sequence alignment, but this repository wil also contain the step 1-3 but not step 4-5 which is a downstream analysis task.

## Installation
### To download dataset from NCBI, please download the program SRA Toolkit
the program should be available at NCBI website or this link https://github.com/ncbi/sra-tools/wiki/01.-Downloading-SRA-Toolkit

### For step 1: Data Preprocessing, ensure that your system have the following programs.
   - trimmomatic
   - fastqc
   - conda
### For step 2: Sequence Alignment, ensure that your system have the following programs.
   - Bowtie2
   - gnu
   - openmpi
   - fastqc
   - conda
### For step 3: Metagenome Assembling, ensure that your system have the following programs.
   - spades
   - QUAST
   - bbmap
   - samtools
   - bedtools2
   - purge
   - conda
