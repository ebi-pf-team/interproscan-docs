Installation requirements
=========================

OS System:
~~~~~~~~~~

``InterProScan`` and all its third party tools are run using the Nextflow workflow system 
and containers, thus allowing the program to be deployed on range of OS and schedulers.

System requirements:
~~~~~~~~~~~~~~~~~~~~

Please note that ``InterProScan`` and the individual member database analyses are
processor and memory intensive.

The minimum specification requirement is a machine with 2 cores and 6 GB
of RAM, which will allow the analysis of a small number of sequences at
a time. However the more resources that are provided the faster the analysis will be, and the more
sequences can be analysed at a time.

Software requirements:
~~~~~~~~~~~~~~~~~~~~~~

- Nextflow (version >=23.10)
    - Install using the `Nextflow documentation <https://www.nextflow.io/docs/latest/install.html>`__
    - Or install via `Conda <https://anaconda.org/bioconda/nextflow>`__ or `Pypi <https://pypi.org/project/nextflow/>`__
- Container runtime
    - ``InterProScan`` includes built in support for Docker, Singularity or Apptainer
    - Using alternative containter runtimes will require creating your `Nextflow profiles <Profiles.html>`__
