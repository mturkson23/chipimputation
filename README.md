# Imputation Workflow h3abionet/chipimputation

[![Build Status](https://travis-ci.org/h3abionet/chipimputation.svg?branch=master)](https://travis-ci.org/h3abionet/chipimputation)

[![Nextflow](https://img.shields.io/badge/nextflow-%E2%89%A519.04.1-brightgreen.svg)](https://www.nextflow.io/)

[![install with bioconda](https://img.shields.io/badge/install%20with-bioconda-brightgreen.svg)](http://bioconda.github.io/)
[![Docker](https://img.shields.io/docker/automated/nfcore/chipimputation.svg)](https://hub.docker.com/r/h3abionet/chipimputation)
![Singularity Container available](
https://img.shields.io/badge/singularity-available-7E4C74.svg)

### Introduction
Imputation is likely to be run in the context of a GWAS, studying population structure, and admixture studies. It is computationally expensive in comparison to other GWAS steps.
The basic steps of the pipeline is description in the diagram below:

![chipimputation pipeline workflow diagram](https://www.h3abionet.org/images/workflows/snp_imputation_workflow.png)

The workflow was developed using [![Nextflow](https://img.shields.io/badge/nextflow-%E2%89%A519.04.1-brightgreen.svg)](https://www.nextflow.io/). The imputation is performed using [Minimac4](https://genome.sph.umich.edu/wiki/Minimac4). It identifies regions to be imputed on the basis of an input file in VCF format, split the regions into small chunks, phase each chunk using the phasing tool [Eagle2](https://data.broadinstitute.org/alkesgroup/Eagle/) and produces output in VCF format that can subsequently be used in a [GWAS]() workflow. It also produce basic plots and reports of the imputation process including the imputation performance report, the imputation accuracy, the allele frequency of the imputed vs of the reference panel and other metrics.    

It comes with docker/singularity containers making installation trivial and results highly reproducible.
This  workflow which was developed as part of the H3ABioNet Hackathon held in Pretoria, SA in 2016.
 - We track our open tasks using github's [issues](https://github.com/h3abionet/chipimputation/issues)

### Documentation
The h3achipimputation pipeline comes with documentation about the pipeline, found in the `docs/` directory:

1. [Installation](docs/installation.md)
2. [Pipeline configuration](docs/configuration/local.md)
    * [Local installation](docs/configuration/local.md)
    * [Adding your own system](docs/configuration/adding_your_own.md)
3. [Running the pipeline](docs/usage.md)
4. [Output and how to interpret the results](docs/output.md)
5. [Troubleshooting](docs/troubleshooting.md)

### Getting started

#### Basic test using test data
This pipeline itself needs no installation - NextFlow will automatically fetch it from GitHub.
You can run the pipeline using test data hosted in github with singularity without have to install or change any parameters. 
```
nextflow run imputation.nf -profile test,singularity
```
- `test` profile will download the testdata from https://github.com/h3abionet/chipimputation_test_data/tree/master/testdata_imputation
- `singularity` profile will download the singularity image from https://quay.io/h3abionet_org/imputation_tools
 
Check for results in `./output`
```
wc -l output/impute_results/*
```

### Setup (native cluster)

#### Headnode
  - [Nextflow](https://www.nextflow.io/) (can be installed as local user)
   - NXF_HOME needs to be set, and must be in the PATH
   - Note that we've experienced problems running Nextflow when NXF_HOME is on an NFS mount.
   - The Nextflow script also needs to be invoked in a non-NFS folder
  - Java 1.8+

#### Compute nodes

- The compute nodes need access to shared storage for input, references, output.
- If you opt to use `singularity` no software installation will be needed.
- Otherwise, the following commands/softwares need to be available in PATH on the compute nodes
  - `minimac4` from [Minimac4](https://genome.sph.umich.edu/wiki/Minimac4)
  - `eagle` from [Eagle](https://data.broadinstitute.org/alkesgroup/Eagle/)
  - `vcftools` from [VCFtools](https://vcftools.github.io/index.html)
  - `bcftools`from [bcftools](https://samtools.github.io/bcftools/bcftools.html)
  - `bgzip` from [htslib](http://www.htslib.org)
  - `python2.7`
  - `R` with the following packages ggplot2, dplyr, data.table, sm, optparse, ggsci
