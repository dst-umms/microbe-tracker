# MAMBA

## MAximum-likelihood-Method Based microbial Analysis
-----------------------------------------------------

***Background:***

Using NGS Whole Genome Sequencing data to elucidate the transmission mechanism and genetic diversity of bacterial isolates has become widespread for the purposes of epidemiological (?) and disease control. To address the complexity involved in analysing such data, some open source tools exist whose work is published in peer reviewed journals. However, the existing tools do not provide an end-to-end solution and demands the end user to figure out many components that need to be put together in order to successfully generate the results. This, in turn, involves significant computational knowledge that many researchers may not have. To address this concern, we have developed an automated, push-button style, end-to-end pipeline called MAMBA: MAximum likelihood-Method Based microbial Analysis. The main goals we have achieved with MAMBA are,

* **Abstracting away the installation:** 
Satisfying the few core dependencies (see dependencies section below), MAMBA r just 3 commands away to fully install all the tools and dependencies required to run the entire pipeline. 

* **Abstracting away the environment:** MAMBA can be installed and executed on any systems with LINUX/UNIX environment, without administrative privileges.

* **Parallel Execution:** MAMBA is built on the backs of “Snakemake” - a python framework to build Bioinformatics pipelines - and thus, makes use of multi-core architecture, be it a local computer or high-performance-computing-cluster, significantly speeding up the processing times.

* **Publication-ready Plots:** MAMBA glues together many tools (published in peer reviewed journals) and generates a stand-alone HTML Report (comprised of embedded images) that can easily be shared over email amongst the investigators.

***

***Prerequisites:***

In order to successfully install MAMBA, following dependencies must be satisfied. Many of these requirements are core part of LINUX/UNIX OS and thus be already available in most cases. Regardless, make sure the versions are correct and most importantly the required library files are present on your system.

* gcc >= 4.8
* g++ >=4.8
* GNU make >= 4.0
* bzip2
* wget
* git
* miniconda3 (https://conda.io/docs/install/quick.html)


In addition, make sure, the following library files are present on your system.

* libbz2.so.1.0
* libjemalloc.so.1
* libXrender.so.1

***

***Installation:***

First, let’s clone the MAMBA code repo, using

`git clone https://github.com/dst-umms/MAMBA.git`

One time only set-up of “conda” virtual environments, using

```
conda env create -f MAMBA/envs/MAMBA.env.yaml
conda env create -f MAMBA/envs/MAMBA_PY.env.yaml
conda env create -f MAMBA/envs/MAMB_R.env.yaml
```

**GATK Software (One time set-up only):**

GATK is a dependency for MAMBA. Due to licensing issues, we could not package this tool as part of MAMBA. If you do not have GATK executable within your system PATH variable, you will need to download the “GenomeAnalysisTK-3.7.tar.bz2” and mention the path to the file in “config.yaml” (see below).

***

***Execution:***

***Kicking MAMBA tyres with test data:***

We have placed test data (subset of Aanensen et. al published data) on Dropbox and can be downloaded using the link,

`http://bit.ly/2twmgsN`

Assuming, you have downloaded the files into MAMBA_TEST_DATA folder, follow the instructions below.

1. You should have a directory structure similar to this at this point.

```
.
├── MAMBA
└── MAMBA_TEST_DATA
```

2. Copy, “config.yml” and “meta.csv” from test_data folder to current working space. Now the directory structure looks like this,

```
├── config.yaml
├── MAMBA
├── MAMBA_TEST_DATA
└── meta.csv
```

3. Feel free to edit “config.yaml” to change memory and cpu values to fit to your system, using your favorite text editor. You should also update “gatk_exec” field with path to GATK bunzipped tar ball only if you don’t have GATK already installed in your system (See “GATK Software” section above to read more on this.)

4. Now, before kicking off the pipeline, there is one thing we need to do, which is, to activate “MAMBA” environment using command,

    `source activate MAMBA`

    You should see at the start of command prompt,

    `(MAMBA) >`

Finally, we are all set to launch the pipeline using,

**Local computer:**

`snakemake -s MAMBA/MAMBA.snakefile --jobs <number_of_cpus_available> --latency-wait 60 >&MAMBA.log &`

**LSF cluster:**

```
snakemake -s MAMBA/MAMBA.snakefile --cluster “bsub -n {threads} -e logs/ -o logs/ -J MAMBA -W 360 -q <queue_name> -R \”rusage[mem={resources[mem]}]\” -R \”span[hosts=1]\” ” --jobs <number of jobs you want to run in parallel> --latency-wait 60 >&MAMBA.log &
```

**Other cluster:** 

Look into documentation to replace values for,

* -n number_of_threads
* -e write logs to a folder
* -o write output to a folder
* -J job_name
* -W wall_clock_time_for_a_running_job
* -q name of the queue
* -mem memory to be given in “MegaBytes”


***

***Running “MAMBA” with your data:***

1. Presuming you have downloaded MAMBA and have a folder with fastq files in the current working space, your first step is to generate "config.yaml" and "meta.csv" files using,

```
source activate MAMBA

python MAMBA/scripts/generate_config.py --fastq_folder /path/to/dir_with_fastqs 1>config.yaml 2>meta.csv

```

**Important Note:** (Input FastQ Filename Convention)

The files ought to be named as _R1.fastq.gz (leftmate) and _R2.fastq.gz (rightmate).

2. Edit "meta.csv" with project specific meta data. (See "Test Run" section above and explore meta.csv for understanding).

3. Edit "config.yaml" file to choose both system level params and pipeline wide params.

4. Launch pipeline using commands mentioned in "Test Run" section above.


***

***MAMBA Output:***

* MAMBA output is present in "analysis" folder in the local workspace.


***

***Contact us:***

Have any questions about MAMBA or wish to report any bugs please email `vangalamaheshh@gmail.com` or open an issue on our github page.  


***








 
