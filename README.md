## Nextflow pipeline for processing of YFISH images

### Requirements
To be able to run this pipeline you need nextflow (version 23.10 or higher) and singularity (tested on version 3.8.6) installed. This does not work on Mac.

The easiest way to install these tools is with conda package manager.  
For example using the following lines (assuming you have conda installed):  
`conda create -n nextflow -c conda-forge -c bioconda nextflow=23.10.0 singularity=3.8*`

If you don't have conda installed yet you can install and initialize it in the following way:  
```
mkdir -p ~/miniconda3
wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh -O ~/miniconda3/miniconda.sh
bash ~/miniconda3/miniconda.sh -b -u -p ~/miniconda3
rm -rf ~/miniconda3/miniconda.sh
~/miniconda3/bin/conda init bash
```

You can then activate the environment by running:  
`conda activate nextflow`

### Testing the pipeline
First clone the pipeline (this will create a folder in your current working directory)  
```
git clone https://github.com/ljwharbers/nextflow-radiantkit
cd nextflow-raidantkit
```

Download sample data from our figshare depositotry
https://doi.org/10.17044/scilifelab.28062272.v1

You can test the pipeline by typing:  
`nextflow run main.nf -profile test`  

This will run it with default parameters and a test samplesheet and dataset that is included in the repository. If this runs without any issues you can run it in your own dataset.
### Running the pipeline with your own dataset
To run the pipeline with your own dataset, there are a few steps to take.

1. Make a samplesheet. You will first need to create a `samplesheet`. An example samplesheet is located inside the repository `/assets/samplesheet.csv` and should be a comma separated file that consists of the following columns: `sample,fastq,barcode,condition` and each row should contain one sample.
2. Adjust `nextflow.config`.  There are some default parameters used and specified in the configuration file and, depending on your most common usecase, it is advisable to change some of these defaults.
   * Check `max_memory` and `max_cpus`. It is important that these do not go above your system values.
   * Go over other parameters defined within the `params { }` section in the config file and change whatever you feel fit.  

Following this you can either change the default parameters in the `nextflow.config` file or supply the parameters related to your own dataset in the command you type. I suggest that you change parameters that won't change much between runs in the `nextflow.config` file, while you specify parameters such as `input` and `output` through the command line.

An example command to run this on your own data could be:  
`nextflow run main.nf –-indir {input_directory} –-outdir /path/to/result/ --dapi {DNA_channel_name} --yfish {yfish_channel_name}`

Finally, if you want to resume canceled or failed runs, you can add the tag `-resume`. Usually, I always use this tag irregardless of if I run something for the first time or if I am resuming a run.
