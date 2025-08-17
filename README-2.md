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
   conda env create -n checkm -f ~/env/env_checkm.yml
   conda env create -n metabat -f ~/env/env_metabat.yml
   conda env create -n dashboard -f ~/env/env_dashboard.yml
```

## SECOND : Running the pipeline
   - All scripts used to run the pipeline are all in "script_main" directory. 
   - The "script_main" directory should be placed in home directory. 
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

### Step 0.1 : Download dataset from NCBI
Put the SRA number of the dataset that you want to download in the "names" array.

```
################################# User input #################################

names=(SRR23958681 SRR23958667 SRR23958665)

##############################################################################
```

Then run the script preferably from home directory.

```
~/script_main/01_sra_main.sh
```

After the download is complete. The rawdata, in fastq file format, will be in "~/01.Rawdata" directory.

### Step 0.2 : Download reference genome of host plant

*** You can download host reference genome from NCBI dataset webpage. ***

Initially, place the plant reference genome file (.fna.gz) into the directory named for the dataset:

```
home
	ref_genome
		ref_{dataset name}
            reference genome file <-- .fna.gz file
```

For example, to add the reference genome file "GCA_004348155.2_Tetep-1.0_genome_genomic.fna.gz" for dataset "SRR9733640":

```
home
	ref_genome
		ref_SRR9733640
		    GCA_004348155.2_Tetep-1.0_genome_genomic.fna.gz
```

After Bowtie 2 (step 2) finishes the indexing phase, additional files will appear in the same directory. 
You don’t need to move or rename anything, just keep the ref_{dataset_name} directory intact:
```
home
	ref_genome
		ref_SRR9733640
            GCA_004348155.2_Tetep-1.0_genome_genomic.1.bt2
            GCA_004348155.2_Tetep-1.0_genome_genomic.2.bt2
            GCA_004348155.2_Tetep-1.0_genome_genomic.3.bt2
            GCA_004348155.2_Tetep-1.0_genome_genomic.4.bt2
			GCA_004348155.2_Tetep-1.0_genome_genomic.fna.gz
            GCA_004348155.2_Tetep-1.0_genome_genomic.rev.1.bt2
            GCA_004348155.2_Tetep-1.0_genome_genomic.rev.2.bt2
```


### Step 1 : Data Preprocessing


