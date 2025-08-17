# Massively Parallel Burrows-Wheeler Alignment for Microbiome Analysis over Plant Genomic Data

# Plant Microbiome Analysis Pipeline
This repository contain the pipeline presented in the paper "Massively Parallel Burrows-Wheeler Alignment for Microbiome Analysis over Plant Genomic Data". In the paper we mainly talk about step 2, sequence alignment, but this repository wil also contain the step 1-3 but not step 4-5 which is a downstream analysis task.

## FIRST : Setup the system
### Download the program "SRA Toolkit".
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

### Create conda environment from env files.
```
   conda env create -n checkm -f env_checkm.yml
   conda env create -n metabat -f env_metabat.yml
   conda env create -n dashboard -f env_dashboard.yml
```

## SECOND : Running the pipeline
   - All scripts used to run the pipeline are all in "script_main" directory. 
   - The "script_main" directory should be placed in home directory. 
   - You should run the script from home directory.
   - All scripts is wrote based on system with PBS job scheduler which is the one that we run on in the paper.

```
   Home
      script_main
         00_dashboard_main.sh
         01_sra_main.sh
         02_trim_main.sh
         03_bowtie_mpi_main.sh
         05_spades_main.sh
         06_Binning_main.sh
```

### Step 0 : Download dataset from NCBI

