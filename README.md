# meioBIOME-metagenomics
C
omputation pipeline for the assembly of host-associated metagenome-assembled genomes and recovery of host single-copy genes

The `meioBIOME/config` folder is where you set up the configurations specific to your HPCC cluster or local compute environment. This folde contains three files that you can edit:

`busco.ini` - set up the directories and parameter setting for BUSCO  
`cluster.yaml` - a central snakemake configuration file (similar to a slurm header) that sets up file paths and node selection (e.g. highmem, batch, etc.) on your local HPCC for all software used in the meioBIOME pipeline
`config.yaml` - a central snakemake configuration file for software parameters and flags for all software used in the meioBIOME pipeline.

The `meioBIOME/workflow` folder contains four subdirectories that contain the databases, environments, rules and scripts needed to run the entire meioBIOME pipeline

`databases` - all the database files and directories that will be called when running the meioBIOME pipeline
`envs`- directory containing YAML files (`filename.yaml`) for the snakemake pipeline. These contain the software and dependencies needed to make conda environments
`rules` - directory containing snakemake files ('filename.smk`) with the actual code blocks that will be used for running data analysis and documenting checkpoint completion throughout the pipeline
`scripts` - custom scripts for data wranging (e.g. processing text files or converting file formats between software tools in the pipeline)

