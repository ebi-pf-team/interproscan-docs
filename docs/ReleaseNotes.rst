Release notes: InterProScan 6.0.0 [beta]
=========================================

Released on 8th July 2025.

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
* Automated downloading of missing metadata and database files.
* Addition of the ``--interpro`` flag to specify the InterPro data version at run time. Defaults to the latest.
* New output file formats:
    * GFF3
    * JSON Lines - unlike the standard JSON containing a single JSON string, this file format contains one JSON string per line, which is advantages when processing many sequences
* Added option to download database data from Globus (can be used when the EBI ftp is down)
* Small internal changes to the post-processing of results to reduce discrepencies with InterProScan version 5

Known issues
^^^^^^^^^^^^

- See the `GitHub Issues page <https://github.com/ebi-pf-team/interproscan6/issues>`__
- Documented on the following page: :ref:`Known issues`.

Reporting issues
^^^^^^^^^^^^^^^^

You found a bug? Or do you want to give us your feedback? Please use
`EMBL EBI's support form <http://www.ebi.ac.uk/support/interproscan>`__.
