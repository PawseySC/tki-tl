FROM centos:7

LABEL maintainer="brian.skjerven@pawsey.org.au"

# Install package dependencies
RUN yum -y update \
    && yum -y install \
         file \
         git \
         sssd-client \
         which \
         wget \
         unzip \
    && yum clean all

# Install Cell Ranger
ARG CELLRANGER_VERSION="2.2.0"

WORKDIR /tmp/cellranger-build

RUN wget -O cellranger-${CELLRANGER_VERSION}.tar.gz \
      "http://cf.10xgenomics.com/releases/cell-exp/cellranger-${CELLRANGER_VERSION}.tar.gz?Expires=1534419718&Policy=eyJTdGF0ZW1lbnQiOlt7IlJlc291cmNlIjoiaHR0cDovL2NmLjEweGdlbm9taWNzLmNvbS9yZWxlYXNlcy9jZWxsLWV4cC9jZWxscmFuZ2VyLTIuMi4wLnRhci5neiIsIkNvbmRpdGlvbiI6eyJEYXRlTGVzc1RoYW4iOnsiQVdTOkVwb2NoVGltZSI6MTUzNDQxOTcxOH19fV19&Signature=CqEpxfMNetU0irJpfXcGiNEk5MO6YMZQ-2ybN8OZ7FoyYho2yEo4aPHHrJ9BdGa94P~173cyzLbOX-oAvdHPxH4isPLsphY5LRHog1P1sAYLcD1-H~njWEcXDlg4~ebItHolxysqgCeC7EG-lWrIAdyvx8WVSLLBZLQZMmbKOrGAYa-5JovYYR~EpUfS3NsrfGlawX2pyj0xcOca6cep~RJhnVMnIvGb~35-TnWO-VyFMY5AAzLMZLfNSx-PBG19H6KvUTd7TYeMrDDRxZTiC5249r05ITLoPQeymlSMnVDRP1zOPsDbRHVOQz~MKSHOPVKqTj7mfgBprkN46bzV-w__&Key-Pair-Id=APKAI7S6A5RYOXBWRPDA" \
    && mv cellranger-${CELLRANGER_VERSION}.tar.gz /opt/ \
    && cd /opt/ \
    && tar -xzvf cellranger-${CELLRANGER_VERSION}.tar.gz \
    && rm -f cellranger-${CELLRANGER_VERSION}.tar.gz \
    && rm -rf /tmp/cellranger-build

# Install bcl2fastq
ARG BCL_ZIP_VERSION="2-20-0"
ARG BCL_RPM_VERSION="2.20.0.422"

WORKDIR /tmp/bcl2fastq-build

RUN wget http://sapac.support.illumina.com/content/dam/illumina-support/documents/downloads/software/bcl2fastq/bcl2fastq2-v${BCL_ZIP_VERSION}-linux-x86-64.zip \
    && unzip bcl2fastq2-v${BCL_ZIP_VERSION}-linux-x86-64.zip \
    && yum -y --nogpgcheck localinstall bcl2fastq2-v${BCL_RPM_VERSION}-Linux-x86_64.rpm \
    && rm -f bcl2fastq2-v${BCL_ZIP_VERSION}-linux.zip \
    && rm -rf /tmp/bcl2fastq-build

# Add cell ranger to path
ENV PATH /opt/cellranger-${CELLRANGER_VERSION}:$PATH
