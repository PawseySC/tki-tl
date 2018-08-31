# tki-tl
Single Cell Analysis pipeline for Timo's project

## Overview

This repo contains Dockerfiles for 2 parts of a single cell analysis pipeline.  The first, `cellranger` is for building and installing 
[Cell Ranger](https://support.10xgenomics.com/single-cell-gene-expression/software/pipelines/latest/installation).  Also included is
the [bcl2fastq](http://sapac.support.illumina.com/sequencing/sequencing_software/bcl2fastq-conversion-software.html) tool.

The second, `sc-pipeline`, contains several R pacakges used as part of the pipeline.  The include:

  * DropletUtils
  * scater
  * Seurat
  * SingleR

The `sc-pipeline` container is built on top of Rocker's [tidyverse](https://hub.docker.com/r/rocker/tidyverse/) container, which can provide a fully functional RStudio server
environment.  R 3.5 is the version installed in the container.

Each of these containers has been built and is available at

https://hub.docker.com/u/tkitl/


## Running Cell Ranger
 
Example data and instructions for running Cell Ranger can found [here](https://support.10xgenomics.com/single-cell-gene-expression/software/pipelines/latest/using/mkfastq).  To run this
example with our Docker container, use the following:

```
docker run -v `pwd`:/data -w /data tkitl/cellranger cellranger mkfastq --id=tiny-bcl --run=cellranger-tiny-bcl-1.2.0 --csv=cellranger-tiny-bcl-simple-1.2.0.csv
```

The `-v` flag specifies which volumes we are mouting into the container.  In this case, we're mounting the current working directory (where we
assume the example data has been downloaded and extracted) to the directory `/data` in the container.  

The `-w` flag tells the container to use the `/data` as the directory to start with once the container has begun execution (as opposed to,
say, changing to the directory or specifying absolute paths in our run command).

The other options are the standard `mkfastq` parameters for Cell Ranger.  You should see output similar to

```
/opt/cellranger-2.2.0/cellranger-cs/2.2.0/bin
cellranger mkfastq (2.2.0)
Copyright (c) 2018 10x Genomics, Inc.  All rights reserved.
-------------------------------------------------------------------------------

Martian Runtime - '2.2.0-v2.3.3'
Serving UI at http://af50141f87d7:34837?auth=hzDqTTexqvgFvlu87G73J0elQ9nzVuRl1LghuWUuNrg

Running preflight checks (please wait)...
Checking run folder...
Checking RunInfo.xml...

...


2018-08-31 06:41:09 [runtime] (run:local)       ID.tiny-bcl.MAKE_FASTQS_CS.MAKE_FASTQS.MERGE_FASTQS_BY_LANE_SAMPLE.fork0.join
2018-08-31 06:41:12 [runtime] (join_complete)   ID.tiny-bcl.MAKE_FASTQS_CS.MAKE_FASTQS.MERGE_FASTQS_BY_LANE_SAMPLE

Outputs:
- Run QC metrics:        null
- FASTQ output folder:   /data/tiny-bcl/outs/fastq_path
- Interop output folder: /data/tiny-bcl/outs/interop_path
- Input samplesheet:     /data/tiny-bcl/outs/input_samplesheet.csv

Waiting 6 seconds for UI to do final refresh.
Pipestance completed successfully!

Saving pipestance info to tiny-bcl/tiny-bcl.mri.tgz
```

## R packages

The `sc-pipeline` container be used to run a full RStudio environment accessible via a web browser, or from the command line using 
an R script to execute commands.  For example:

```
docker run tkitl/sc-pipeline R --version                                                                                                                                                   master ➦ a3973b5 ◼

R version 3.5.1 (2018-07-02) -- "Feather Spray"
Copyright (C) 2018 The R Foundation for Statistical Computing
Platform: x86_64-pc-linux-gnu (64-bit)
```

To run this container locally, a few more options are required:

```
docker run -d -v /home/data:/home/rstudio/data -p 8787:8787 tkitl/sc-pipeline
```
This will run an RStudio server that can accessed via a web browser at http://localhost:8787.  The default username and password are both `rstudio` (they can be
changed with the Docker options `-e USER=<username>` and `-e PASSWORD=<password>`.

The same RStudio container can be used in a remote fashion as well.  The `docker run` command above can be run on a remote server (e.g. an Nimbus
instance), and can then be accessed via http://XXX.XXX.XXX.XXX:8787 where `XXX.XXX.XXX.XXX` is the static IP of your VM.

A more advanced method of running the RStudio container can be found at https://github.com/PawseySC/rstudio-nginx.  This setup allows
for users to run an RStudio container behind a secure reverse proxy server.  The `sc-pipeline` container can be used with the the `docker-compose.yml`
file available in that repo, and for more instructions, refer to that `rstudio-nginx` repo.
