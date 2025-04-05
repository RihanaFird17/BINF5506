FROM continuumio/miniconda3
 
WORKDIR /code
 
RUN apt update
 
RUN apt upgrade -y
 
RUN apt install tree wget git ca-certificates -y
RUN update-ca-certificates

# RUN apt install -y openssl
# RUN conda config --set ssl_verify false

# Configure Conda to use the correct channels
RUN conda update -n base -c defaults conda

RUN conda config --set channel_priority strict && \
    conda config --add channels defaults && \
    conda config --add channels bioconda && \
    conda config --prepend channels conda-forge

# Install bioinformatics tools
RUN conda install -c bioconda -c conda-forge \
    entrez-direct sra-tools fastqc samtools bwa gatk4 snpEff snakemake -y 

RUN conda install -c conda-forge awscli curl -y
RUN conda update sra-tools curl openssl -y
RUN conda clean --all -y

RUN conda install -c conda-forge boto3 -y

# Copy AWS credentials into the container
COPY .aws/credentials /root/.aws/credentials

# Copy the SnakeMake script into the container
# COPY genomic_analysis_script.sh /code/genomic_analysis_script.sh
COPY Snakefile /code/Snakefile

# Set the default command to run the pipeline
# CMD ["bash", "/code/genomic_analysis_script.sh"]
# CMD ["snakemake", "--cores", "4"]