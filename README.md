# meioBIOME-metagenomics
Computation pipeline for the assembly of host-associated metagenome-assembled genomes and recovery of host single-copy genes

<img width="562" height="758" alt="Screenshot 2026-06-02 at 1 08 28 PM" src="https://github.com/user-attachments/assets/a6542a32-3878-4211-ad8e-f96f74bb79f0" />

The `meioBIOME/config` folder is where you set up the configurations specific to your HPCC cluster or local compute environment. This folde contains three files that you can edit:

* `busco.ini` - set up the directories and parameter setting for BUSCO  
* `cluster.yaml` - a central snakemake configuration file (similar to a slurm header) that sets up file paths and node selection (e.g. highmem, batch, etc.) on your local HPCC for all software used in the meioBIOME pipeline
* `config.yaml` - a central snakemake configuration file for software parameters and flags for all software used in the meioBIOME pipeline.

The `meioBIOME/workflow` folder contains four subdirectories that contain the databases, environments, rules and scripts needed to run the entire meioBIOME pipeline

* `databases` - all the database files and directories that will be called when running the meioBIOME pipeline
* `envs`- directory containing YAML files (`filename.yaml`) for the snakemake pipeline. These contain the software and dependencies needed to make conda environments
* `rules` - directory containing snakemake files ('filename.smk`) with the actual code blocks that will be used for running data analysis and documenting checkpoint completion throughout the pipeline
* `scripts` - custom scripts for data wranging (e.g. processing text files or converting file formats between software tools in the pipeline)

# Installing meioBIOME 

### Step 1: Install Miniforge3 (or load the module on your local HPCC)
1. On the University of Georga GACRC, you can load the module using: `ml Miniforge3`

### Step 2: Install snakemake
1. On the University of Georgia GACRC, first request an interactive node from the Sapelo2 login node: `interact --mem=16G`
2. Create a new conda environment for **snakemake version 7**: `mkdir snakemake-7.32.0`
3. Load the miniforge module `ml Miniforge3`
4. Crate the conda environment (NOTE: you will get a warning that the directory already exists - enter **yes** to continue): `conda create -p snakemake-7.32.0`
5. Activate the conda environment (NOTE: you may need to put the full file path to activate the conda environment correctly, depending on your HPCC or local computer setup): `source activate /FULL-PATH-HERE/snakemake-7.32.0`
6. Install snakemake within your newly created conda environment: `conda install bioconda::snakemake==7.32.0`

### Step 3: Download the meioBIOME pipeline

Clone the github directory:
`git clone ADDRESS`

### Step 4: Install Databases
Install the following databases in the `workflow/databases` directory:
1. Adapter files for Trimmomatic [https://github.com/usadellab/trimmomatic]
2. CheckM2 Database [https://github.com/chklovski/CheckM2]
3. GTDBTK Database [https://github.com/ecogenomics/gtdbtk]
4. Kraken Database [https://benlangmead.github.io/aws-indexes/k2]
5. BUSCO v12 for your appropriate host organism [https://busco-data.ezlab.org/v6/data/lineages/]
6. CompleteBin Database [https://github.com/zoubohao/CompleteBin]
7. MagPurify [https://zenodo.org/records/3688811]

### Step 5: Edit the Config and Cluster YAML Files
Edit the following information: 
1. Input paths: 
2. Output paths: 
3. scratch path:
4. Paths to each database

### Step 6: Edit the script file and Run
The primary input for the MeioBIOME pipeline v1.0 is a directory that contains raw non-interleaved paired-end sequence files using the following specific, yet simple, naming convention: `samplename_R[1/2].fastq.gz` (Figure 1). In addition, a configuration file allows for the customization of certain parameters throughout the workflow, while the cluster configuration file allows the pipeline to be easily parallelized and run on a high-performance computing cluster.  At each step, the pipeline utilizes Conda to install the necessary open-source computational tools. The pipeline can be easily run using five commands in the following order: 

1. `snakemake --until reads_quality_trim --cluster-config cluster.yaml  --snakefile snakefile`
2. `snakemake --until metagenome_assembly --cluster-config cluster.yaml  --snakefile snakefile`
3. `snakemake --until host_genome_skim --cluster-config cluster.yaml  --snakefile snakefile`
4. `snakemake --until microbiome_bins --cluster-config cluster.yaml  --snakefile snakefile`
5. `snakemake --until microbiome_bins_quality --cluster-config cluster.yaml  --snakefile snakefile`
6. `snakemake --until microbiome_bins_quant --cluster-config cluster.yaml  --snakefile snakefile`








