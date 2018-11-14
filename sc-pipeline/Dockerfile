FROM rocker/tidyverse:3.5

RUN apt-get update -qq && apt-get -y --no-install-recommends install \
      autoconf \
      automake \
      default-jre \
      g++ \
      gcc \
      gfortran \
      hdf5-tools \
      libhdf5-dev \
      make \
      && apt-get clean all \
      && rm -rf /var/lib/apt/lists/*

RUN mkdir -p $HOME/.R
COPY Makevars /root/.R/Makevars

RUN Rscript -e "library('devtools')" \
      -e "install.packages('BiocManager')" \
      -e "source('http://bioconductor.org/biocLite.R')" \
      -e "biocLite('DropletUtils')" \
      -e "biocLite('GSEABase')" \
      -e "biocLite('GSVA')" \
      -e "BiocManager::install('scater')" \
      -e "install.packages('robustbase', repos='http://R-Forge.R-project.org')" \
      -e "install_github('dviraran/SingleR')" \
      && rm -rf /tmp/downloaded_packages

CMD ["/bin/bash"]
