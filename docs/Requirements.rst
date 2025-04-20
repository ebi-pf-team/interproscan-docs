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

* 3 cores
* 8 GB RAM

``InterProScan`` is processor and memory intensive.
The more resources that are provided the faster the analysis will be, and the more
sequences that can be analysed at a time.

Software requirements:
~~~~~~~~~~~~~~~~~~~~~~

* Nextflow (version >=24.10.4)
* A container runtime:
    * Docker (version >= 24.0.5)
    * Singularity (version >= 4.2.0)
    * Apptainer (version >= 1.3.4)

Licenses and additional data from their respective authors are required to run ``Phobius``, ``SignalP`` and ``DeepTMHMM``.
