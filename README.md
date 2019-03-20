# Metagenomics Workflows
Workflows for metagenomic sequence data processing and analysis.  Further documentation found in each workflow folder.

### Long read assembly has been moved to a new repository: [Lathe](https://github.com/elimoss/lathe)

# Installing workflows

First, install [miniconda3](https://conda.io/en/latest/miniconda.html)

Then install snakemake.  This can be done with the following.

```
conda install snakemake
snakemake --version #please ensure this is >=5.4.3
```

Next, clone this github directory to some location where it can be stored permanently.  Remember to keep it updated with `git pull`.

```
git clone https://github.com/elimoss/metagenomics_workflows.git
```

Snakemake does not have native support for SLURM. Instructions to enable Snakemake to schedule cluster jobs with SLURM can be found at https://github.com/bhattlab/slurm


# bin_label_and_evaluate

Snakemake workflow for aligning, binning, classifying and evaluating a
metagenomic assembly.

Before running this workflow, please do the following:

	source activate mgwf #activate the environment
	cd <checkm data directory of your choice>
	wget https://data.ace.uq.edu.au/public/CheckM_databases/checkm_data_2015_01_16.tar.gz #download checkm databases
	tar -zxf checkm_data_2015_01_16.tar.gz
	checkm data setRoot #set the location for checkm data and wait for it to initialize

## Inputs
### Alter config.yaml to provide the following:
 * **Assembly**: Sequence to bin. Fasta format.

 * **Sample**: names the output directory.

 * **Reads 1, Reads 2**: forward and reverse reads in fastq or fastq.gz format.

 * **Krakendb**: Kraken2 database with which to classify asssembly contigs.

 * **Read length**: read length.

Known problems: occasionally fails after binning step. Just re-run snakemake.  This is a problem with dynamic job scheduling, and will hopefully be fixed in a future snakemake update.

to run, please use the following:

```
snakemake -s path/to/metagenomics_workflows/bin_label_and_evaluate/Snakefile --configfile path/to/modified_config.yaml --restart-times 0 --keep-going
#--profile scg #run this on a cluster.  this is highly recommended.  See above.
```

# assembly_comparison_circos
Snakemake workflow for visualizing assemblies of a particular genome across conditions and time points.  Calls out pre-identified sequences, highlights selected contigs.
