FROM 4dndcic/ubuntu16.04-miniconda-python36:v1
MAINTAINER Soo Lee (duplexa@gmail.com), Michele Berselli (berselli.michele@gmail.com)

# general updates & installing necessary Linux components
RUN apt-get update -y && apt-get install -y \
    bzip2 \
    gcc \
    g++ \
    git \
    less \
    libncurses-dev \
    make \
    time \
    unzip \
    vim \
    wget \
    zlib1g-dev \
    liblz4-tool \
    libbz2-dev \
    liblzma-dev \
    locales \
    parallel \
    autoconf \
    autogen \
    pkg-config \
    tabix

WORKDIR /usr/local/bin

# gatk4 & picard & python libraries
RUN conda install -c bioconda -y gatk4==4.1.2.0 picard==2.20.2 \
    pysam==0.15.3 bitarray==1.2.1 pytabix==0.0.2 h5py==2.10.0 numpy==1.18.1 pyfaidx==0.5.8 python=3.6.8 && \
    conda clean -a -y -f

# granite
RUN git clone https://github.com/dbmi-bgm/granite.git && \
    cd granite && \
    git checkout 695d590 && \
    rm -rf tests && \
    python setup.py install && \
    cd ..

# R
RUN apt-get install -y apt-transport-https software-properties-common  # to add add-apt-repository and to avoid method driver could not be found error
RUN apt-key adv --keyserver keyserver.ubuntu.com --recv-keys E298A3A825C0D65DFD57CBB651716619E084DAB9 && \
    add-apt-repository 'deb [arch=amd64,i386] https://cran.rstudio.com/bin/linux/ubuntu xenial-cran35/' && \
    apt-get update -y && apt-get install -y \
    libcurl4-openssl-dev \
    libssl-dev \
    r-base \
    r-base-dev

# samtools
RUN wget https://github.com/samtools/samtools/releases/download/1.9/samtools-1.9.tar.bz2 && \
    tar -xjf samtools-1.9.tar.bz2 && \
    cd samtools-1.9 && \
    ./configure && \
    make && \
    cd .. && \
    ln -s samtools-1.9 samtools

# bedtools
RUN wget https://github.com/arq5x/bedtools2/releases/download/v2.29.0/bedtools-2.29.0.tar.gz && \
    tar -zxvf bedtools-2.29.0.tar.gz && \
    cd bedtools2 && \
    make && \
    cd .. && \
    ln -s bedtools2 bedtools

# paths
ENV PATH=/usr/local/bin/samtools/:$PATH
ENV PATH=/usr/local/bin/bedtools/bin/:$PATH

# supporting UTF-8
RUN locale-gen "en_US.UTF-8" && update-locale LC_ALL="en_US.UTF-8"
ENV LC_ALL=en_US.UTF-8

CMD ["bash"]