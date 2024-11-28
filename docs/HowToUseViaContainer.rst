How to Use InterProScan via Container
=====================================

InterProScan can be used via Docker or Singularity to simplify the installation and execution process in different computing environments.

Before using InterProScan 5 via container, make sure you are using Linux operating system and you have Docker or Singularity installed and running on your system.

The image (https://hub.docker.com/r/interpro/interproscan) does not include the data required to run InterProScan, which needs to be downloaded separately.

Get InterProScan data
~~~~~~~~~~~~~~~~~~~~~

For the latest InterProScan release (5.72-103.0), data can be downloaded with the following command:

::

    curl -O http://ftp.ebi.ac.uk/pub/software/unix/iprscan/5/5.72-103.0/alt/interproscan-data-5.72-103.0.tar.gz

We always recommend verifying your downloads for corruption:

::

    curl -O http://ftp.ebi.ac.uk/pub/software/unix/iprscan/5/5.72-103.0/alt/interproscan-data-5.72-103.0.tar.gz.md5
    md5sum -c interproscan-data-5.72-103.0.tar.gz.md5

Finally, extract data files from the downloaded archive:

::

    tar -pxzf interproscan-data-5.72-103.0.tar.gz

The data will be extracted to interproscan-5.72-103.0/data.


Using Docker
~~~~~~~~~~~~

Pull the image:
::
    docker pull interpro/interproscan

Example:
^^^^^^^^

Create the directories for temporary and output files, respectively:

::

    mkdir input temp output

Copy you input file(s) to the ``input`` directory:

::

    # Escherichia coli K-12 proteome
    wget -O input/e-coli.fa 'https://rest.uniprot.org/uniprotkb/stream?format=fasta&query=%28proteome%3AUP000000625%29'

Run the image:

::

    docker run --rm \
        -v $PWD/interproscan-5.72-103.0/data:/opt/interproscan/data \
        -v $PWD/input:/input \
        -v $PWD/temp:/temp \
        -v $PWD/output:/output \
        interpro/interproscan:5.72-103.0 \
        --input /input/e-coli.fa \
        --output-dir /output \
        --tempdir /temp \
        --cpu 8

Using Singularity
~~~~~~~~~~~~~~~~~

Pull the image:
::

    singularity pull docker://interpro/interproscan:latest

Example:
^^^^^^^^

Create the directories for temporary and output files, respectively:

::

    mkdir input temp output

Copy you input file(s) to the ``input`` directory:

::

    # Escherichia coli K-12 proteome
    wget -O input/e-coli.fa 'https://rest.uniprot.org/uniprotkb/stream?format=fasta&query=%28proteome%3AUP000000625%29'

Run the image:

::

    singularity exec \
        -B $PWD/interproscan-5.72-103.0/data:/opt/interproscan/data \
        -B $PWD/input:/input \
        -B $PWD/temp:/temp \
        -B $PWD/output:/output \
        interproscan_latest.sif \
        /opt/interproscan/interproscan.sh \
        --input /input/e-coli.fa \
        --disable-precalc \
        --output-dir /output \
        --tempdir /temp \
        --cpu 8

**NOTE**: In Singularity, it is necessary to provide the full path to the interproscan.sh script.
