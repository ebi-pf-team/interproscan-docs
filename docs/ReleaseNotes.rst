Release notes: InterProScan 6.0.0
==================================

Released on 19th April 2025.

What’s new
~~~~~~~~~~

Data update
^^^^^^^^^^^

* Synchronised with `InterPro version 105.0 <http://www.ebi.ac.uk/interpro/release_notes>`__.

Software updates
^^^^^^^^^^^^^^^^

* Migration to a Nextflow workflow manager.
* Support for Linux, MacOS and Windows OS as well as SLURM and LSF schedulers.
* Containerised deployment, supporting Docker, Apptainer and Singularity.
* Decoupled software and data release.
* Implementation of the ``--download`` option to automate downloading missing metadata and database files.
* Addition of the ``--interpro`` flag to specify the InterPro data version at run time. Defaults to the latest.

Known issues
^^^^^^^^^^^^

- See the `GitHub Issues page <https://github.com/ebi-pf-team/interproscan6/issues>`__
- Documented on the following page: :ref:`Known issues`.

Reporting issues
^^^^^^^^^^^^^^^^

You found a bug? Or do you want to give us your feedback? Please use
`EMBL EBI's support form <http://www.ebi.ac.uk/support/interproscan>`__.
