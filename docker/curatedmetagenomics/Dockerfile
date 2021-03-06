FROM ubuntu

ARG DEBIAN_FRONTEND=noninteractive
RUN apt-get update
RUN apt-get -y upgrade
RUN apt-get install -y python3
RUN apt-get install -y curl
RUN apt-get install -y wget

## Install conda
RUN curl -O https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh
RUN sh Miniconda3-latest-Linux-x86_64.sh -b -p /usr/local/miniconda3
RUN rm Miniconda3-latest-Linux-x86_64.sh
ENV PATH=/usr/local/miniconda3/bin:$PATH
RUN conda config --add channels defaults
RUN conda config --add channels bioconda
RUN conda config --add channels conda-forge

## install metaphlan3, humann3, and the curatedMetagenomics pipeline
RUN conda update conda
RUN conda install -c bioconda metaphlan=3
RUN conda update metaphlan
RUN conda install -c biobakery humann
RUN conda install bowtie2=2.3.5.1
RUN pip install curatedmetagenomicspipeline

## install sratoolkit
RUN mkdir /tmp/sra
WORKDIR /tmp/sra

RUN curl -O https://ftp-trace.ncbi.nlm.nih.gov/sra/sdk/2.10.5/setup-apt.sh
RUN /bin/bash setup-apt.sh
ENV PATH=/usr/local/ncbi/sra-tools/bin:$PATH

## interactive user settings workaround
COPY usersettings /root/.ncbi/user-settings.mkfg

RUN rm -rf /tmp/sra

## display versions
RUN fastq-dump --version
RUN metaphlan --version
RUN humann --version

## root directory for databases
ENV mpa_dir=/usr/local/miniconda3/lib/python3.7/site-packages/metaphlan
ENV hnn_dir=/usr/local/miniconda3/lib/python3.7/site-packages/humann
WORKDIR /root
