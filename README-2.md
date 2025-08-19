# Massively Parallel Burrows-Wheeler Alignment for Microbiome Analysis over Plant Genomic Data

# Plant Microbiome Analysis Pipeline
This repository contain the pipeline presented in the paper "Massively Parallel Burrows-Wheeler Alignment for Microbiome Analysis over Plant Genomic Data". In the paper we mainly discuss about step 2, sequence alignment. However, this repository also includes steps 1–3, but not steps 4–5, which involve downstream analysis tasks.

## 🔧 FIRST : Setup the system
### Download the program "SRA Toolkit".
the program should be available at NCBI website or this link https://github.com/ncbi/sra-tools/wiki/01.-Downloading-SRA-Toolkit  

	- Make sure that you can call the SRA Toolkit program from home directory.  
	- In this code we use program version "sratoolkit.3.1.1-centos_linux64". If you want to use other version, please change the program name in ~/script/01_sra_sub.sh file (around line 35) accordingly.  


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
### For Binning step, ensure that your system have the following programs.
   - QUAST
   - bbmap
   - samtools
   - bedtools2
   - purge
   - conda
### For Dashboard step, ensure that your system have the following programs.
   - conda


### Create conda environment from env files.
```
   conda env create -n checkm -f ~/env/env_checkm.yml
   conda env create -n metabat -f ~/env/env_metabat.yml
   conda env create -n dashboard -f ~/env/env_dashboard.yml
```

## 🚀 SECOND : Running the pipeline
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
Enter the following parameters for each jobs. The parameter-array values in the same index position will be use for the same job. 

✅ Since each jobs will run with only one dataset, this script will help you run multiple dataset at the same time without the extra step to "qsub" each job by yourself.

```
################################# User input #################################

# Define parameter arrays
names=(SRR23958681 SRR23958667 SRR23958665)             # Sample names
memories=(80 80 80)                                     # Memory in GB
times=("10:00:00" "80:00:00" "80:00:00")                # Walltime
ppns=(16 16 16)                                         # Processors per node

##############################################################################
```

Then run the script preferably from home directory.

```
~/script_main/01_sra_main.sh
```

After the download is complete. The rawdata, in fastq file format, will be in directory:
```
~/01.Rawdata
```

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



### Step 1 : Data Preprocessing using Trimmomatics
Enter the following parameters for each jobs. The parameter-array values in the same index position will be use for the same job. 
```
################################# User input #################################

# Define parameter arrays
names=(SRR23958681 SRR23958667 SRR23958665)             # Sample names
memories=(80 80 80)                                     # Memory in GB
times=("10:00:00" "80:00:00" "80:00:00")                # Walltime
ppns=(16 16 16)                                         # Processors per node
threads=(16 16 16)                                      # Threads for trimmomatic

############################################################################## 
```

Then run the script preferably from home directory.

```
~/script_main/02_trim_main.sh
```

The result files will be in directory:
```
~/02.Trim_{Sample name}
```


### Step 2 : Sequence Alignment using Bowtie2 with our proposed improvement method
Enter the following parameters for each jobs. The parameter-array values in the same index position will be use for the same job. 
```
################################# User input #################################

# Define parameter arrays
names=(SRR23958681 SRR23958667 SRR23958665))         # Sample names
memories=(80 80 80)                                  # Memory in GB
times=("24:00:00" "12:00:00" "12:00:00")             # Walltime
ppns=(32 32 32)                                      # Processors per node
procs=(32 32 32)									 # Processors per process
threads=(4 4 4)										 # Threads per process

############################################################################## 
```

Then run the script preferably from home directory.

```
~/script_main/03_bowtie_mpi_main.sh
```

The result files will be in directory:
```
~/03.Mapping-genome-bowtie_MPI_{Sample name}
```


### Step 3 : Metagenome Assembling using metaSPAdes
Enter the following parameters for each jobs. The parameter-array values in the same index position will be use for the same job. 
```
################################# User input #################################

# Define parameter arrays
names=(SRR23958681 SRR23958667 SRR23958665)             # Sample names
memories=(80 80 80)                                     # Memory in GB
times=("80:00:00" "80:00:00" "80:00:00")                # Walltime
ppns=(16 16 16)                                         # Processors per node
threads=(16 16 16)                                      # Threads for metaSPAdes

############################################################################## 
```

Then run the script preferably from home directory.

```
~/script_main/05_spades_main.sh
```

The result files will be in directory:
```
~/05.assembly_{Sample name}
```


### Binning
Enter the following parameters for each jobs. The parameter-array values in the same index position will be use for the same job. 
```
################################# User input #################################

# Define parameter arrays
names=(SRR23958681 SRR23958667 SRR23958665)             # Sample names
memories=(80 80 80)                                     # Memory in GB
times=("10:00:00" "12:00:00" "24:00:00")                # Walltime
ppns=(16 16 16)                                         # Processors per node
threads=(16 16 16)                                      # Threads for program

############################################################################## 
```

Then run the script preferably from home directory.

```
~/script_main/06_Binning_main.sh
```

The result files will be in directory:
```
~/05.assembly_{Sample name}/06.coverage
~/05.assembly_{Sample name}/07.binning
~/05.assembly_{Sample name}/08.checkM
```


### Creating dashboard file
Enter the following parameters for each jobs. The parameter-array values in the same index position will be use for the same job. 
```
################################# User input #################################

names=(SRR23958681 SRR23958667 SRR23958665)             # Sample names

##############################################################################
```

Then run the script preferably from home directory.

```
~/script_main/00_dashboard_main.sh
```

The result files will be in directory:
```
~/dashboard_files_{Sample name}
```

