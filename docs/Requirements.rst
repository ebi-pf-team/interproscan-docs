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
* 8 GB RAM

``InterProScan`` is processor and memory intensive.
The more resources that are provided the faster the analysis will be, and the more
sequences that can be analysed at a time.

Software requirements:
~~~~~~~~~~~~~~~~~~~~~~

* Nextflow (version 25.04.6 or later)
* A container runtime:
    * Docker (version 25.4.0 or later)
    * Singularity (version 4.2.0 or later)
    * Apptainer (version 1.3.4 or later)

Licenses and additional data from their respective authors are required to run ``DeepTMHMM``, ``Phobius``, and ``SignalP``.
