How to Use InterProScan via Container
==================================

InterProScan can be used via Docker or Singularity to simplify the installation and execution process in different computing environments.

Before using InterProScan 5 via container, make sure you are using Linux operating system and you have Docker or Singularity installed and running on your system.

The image (https://hub.docker.com/r/interpro/interproscan) does not include the data required to run InterProScan, which needs to be downloaded separately.

Get InterProScan data
~~~~~~~~~~~~~~~~~~~~~~~~~

For the latest InterProScan release (5.67-99.0), data can be downloaded with the following command:

::

    curl -O http://ftp.ebi.ac.uk/pub/software/unix/iprscan/5/5.67-99.0/alt/interproscan-data-5.67-99.0.tar.gz

We always recommend verifying your downloads for corruption:

::

    curl -O http://ftp.ebi.ac.uk/pub/software/unix/iprscan/5/5.67-99.0/alt/interproscan-data-5.67-99.0.tar.gz.md5
    md5sum -c interproscan-data-5.67-99.0.tar.gz.md5

Finally, extract data files from the downloaded archive:

::

    tar -pxzf interproscan-data-5.67-99.0.tar.gz

The data will be extracted to interproscan-5.67-99.0/data.


Using Docker
~~~~~~~~~~~~~~~~~~~~~~~~~

Pull the image:
::
    docker pull interpro/interproscan

Example:
^^^^^^^^^^

Create the directories for temporary and output files, respectively:

::

    mkdir temp output

Run the image:

::

    docker run --rm \
    -v $PWD/interproscan-5.67-99.0/data:/opt/interproscan/data \
    -v $PWD/output:/output \
    -v $PWD/temp:/temp \
    interpro/interproscan:5.67-99.0 \
    --input /path/to/your/file.fasta \
    --disable-precalc \
    --output-dir /output \
    --tempdir /temp \
    --cpu 16

Using Singularity
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Pull the image:
::

    singularity pull docker://interpro/interproscan:latest

Example:
^^^^^^^^^^

Create the directories for temporary and output files, respectively:

::

    mkdir temp output

Run the image:

::

    singularity exec
        -B $PWD/interproscan-latest/data:/opt/interproscan/data \
        -B $PWD/output:/output \
        -B $PWD/temp:/temp \
        interproscan_latest.sif \
        /opt/interproscan/interproscan.sh \
        --input /path/to/your/file.fasta \
        --disable-precalc \
        --output-dir /output \
        --tempdir /temp \
        --cpu 16 \

**NOTE**: In Singularity, it is necessary to provide the full path to the interproscan.sh script.
