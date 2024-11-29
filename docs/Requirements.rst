Installation requirements
=========================

OS System:
~~~~~~~~~~

* Linux
* MaxOS
* Windows

System requirements:
~~~~~~~~~~~~~~~~~~~~

**Minimum (for the analysis of a small number of sequences):**
* 2 cores
* 6 GB RAM

`InterProScan`` and the individual member database analyses are processor and memory intensive.
The more resources that are provided the faster the analysis will be, and the more
sequences that can be analysed at a time.

Software requirements:
~~~~~~~~~~~~~~~~~~~~~~

* Nextflow (version >=24.04)
    * `Nextflow documentation <https://www.nextflow.io/docs/latest/install.html>`__
    * `Conda <https://anaconda.org/bioconda/nextflow>`__
    * `Pypi <https://pypi.org/project/nextflow/>`__
* Container runtime
    * ``InterProScan`` includes built in support for Docker, Singularity or Apptainer
    * Using alternative container runtimes will require creating your own `Nextflow profiles <Profiles.html>`__
