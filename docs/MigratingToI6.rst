Migrating from InterProScan Version 5 to Version 6
==================================================

New system requirements
~~~~~~~~~~~~~~~~~~~~~~~

* Nextflow (version >=24.10.4)
* A container runtime
* Linux, MacOS or Windows

Downloading and using the databases
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Automated database downloads
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

``InterProScan`` 6 automatically downloads any missing database files, so you no longer need to download and prepare
these files in advance.

Specify the data directory
^^^^^^^^^^^^^^^^^^^^^^^^^^

Unlike version 5 which presumes the database data is located at ``./data``, ``InterProScan`` 6
**must** be directed to the data directory when running any of the built-in (non-licensed) applications using the
``--datadir`` flag.

Specify the InterPro version (best practise)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

``InterProScan`` 6 will automatically download the latest version of InterPro. For reproducibility and reproducion
of analyses, use the ``--interpro`` flag to specify InterPro release to use [Default: ``latest``].

.. code-block:: bash

    nextflow run ebi-pf-team/interproscan6 \
        -profile docker\
        --input tests/data/test_prot.fa \
        --datadir <path-to-the-data-dir> \
        --interpro 104.0

Running InterProScan: Flag changes
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Support only for long-name (double dashed) flags
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

``InterProScan`` 6 only supports long-name (double dashed) flags, therefore, all flags from ``InterProScan``
5 that are supported in ``InterProScan`` 6 must be convereted to their long name.

New flags
^^^^^^^^^

* ``-profile`` - Specify the run time profiles. Typically the container runtime (e.g. ``docker``), and when not running locally the executor (e.g. ``slurm``). Note this option uses a single dash.
* ``--nucleic`` - ``InterProScan`` 6 defaults to analysing protein sequences. To analyse an input FASTA file of nucleotide sequences use the ``--nucleic`` flag.
* ``--interpro`` - Specify the InterPro version to use.

Renamed flags
^^^^^^^^^^^^^

+-------------------------+------------------------+------------------------------------------------------------+
| InterProScan 5          | InterProScan 6         | Description                                                |
+=========================+========================+============================================================+
| ``--disable-precalc``   | ``--no-matches-api``   | Skip retrieving precalculated matches                      |
+-------------------------+------------------------+------------------------------------------------------------+
| ``--output-dir``        | ``--outdir``           | Output directory                                           |
+-------------------------+------------------------+------------------------------------------------------------+
| ``--output-file-base``  | ``--outprefix``        | Specify the base name for output files                     |
+-------------------------+------------------------+------------------------------------------------------------+
| ``--excl-applications`` | ``--skip-annotations`` | Comma-separated list of analyses to exclude                |
+-------------------------+------------------------+------------------------------------------------------------+
| ``--seqtype``           | ``--nucleic``          | Analyse nucleotide sequences                               |
+-------------------------+------------------------+------------------------------------------------------------+

Note, ``InterProScan`` 6 will build the directory (including parents) if it does not already exist.

Deprecated flags
^^^^^^^^^^^^^^^^

* ``-cpu,--cpu``
* ``-b,--output-file-base``
* ``-dra,--disable-residue-annot``
* ``-etra,--enable-tsv-residue-annot``
* ``-incldepappl,--incl-dep-applications``
* ``-iprlookup,--iprlookup``
* ``-ms,--minsize``
* ``-o,--outfile``
* ``-T,--tempdir`` [use the Nextflow ``-work-dir`` flag. Note the single dash]
* ``-verbose,--verbose``
* ``-version,--version`` [the version is always printed out to the terminal when ``InterProScan`` 6 launches]
* ``-vl,--verbose-level``
* ``-vtsv,--output-tsv-version``

Application/Member db name aliases
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The formating of all application names from ``InterProScan`` 5 are accepted in ``InterProScan`` 6.

SignalP and TMHMM update
^^^^^^^^^^^^^^^^^^^^^^^^

TMHMM has been upgraded to DeepTMHMM in ``InterProScan`` 6, TMHMM is not supported, and the application name has
changed from ``tmhmm`` to ``deeptmhmm``.

SignalP has been upgraded to version 6 in ``InterProScan`` 6, and the application names for ``signalp_gram_negative`` and
``signalp_gram_positive`` have been combined and changed to ``signalp_prok`` for prokaryotic sequences.

``InterProScan`` 6 supports GPU acceleration of these tools, see the
`Installing Licensed Applications page <InstallingLicensedApps.rst>`__ for more information.

Running on a cluster or cloud
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Unlike ``InterProScan`` 5, ``InterProScan`` 6 does not require reconfiguration to run on a cluster or a cloud.

By default ``InterProScan`` 6 runs in local mode, to run on a cluster or cloud use the ``--profile`` flag to specify
the executor.

At the moment, ``InterProScan`` provides only built-in support for the SLURM and LSF schedulers.
See the `profiles page <Profiles.html>`__ documentation for more information on
using alternative system schedulers.

Output files
~~~~~~~~~~~~

For improved parsing of large output files containing many sequences and matches, ``InterProScan`` 6 can produce
a JSON line output file, see the `Output Formats page <OutputFormats.rst>`__ for more information.

The content and structure of the TSV, JSON and GFF3 files are the same between versions 5 and 6, except for:

* Gene3D and FunFam have been renamed to ``Cath-Gene3D`` and ``Cath-FunFam``.
* The InterPro version and ``InterProScan`` version have been separated:

.. code-block:: json

    {
        "interproscan-version": "6.0.0-beta",
        "interpro-version": "106.0",
        "results": []
    }


XML Schema changes
^^^^^^^^^^^^^^^^^^

* Root node renamed from ``protein-matches`` to ``results`` (matching the JSON structure).
* All match nodes and location node names have been renamed from their application-based naming to generic ``match`` and ``location`` nodes.

Nucleotide output files
^^^^^^^^^^^^^^^^^^^^^^^

In ``InterProScan``` 5, when analyzing a FASTA file containing nucleotide sequences, identical open reading frames (ORFs)
from different parent sequences were only reported under one of those nucleotide sequences. This made it difficult
to trace all associated hits without closely examining the data.

In ``InterProScan``` 6, this issue has been resolved. Now, all matches from all ORFs are correctly associated with
each of their parent nucleotide sequences, making the results easier to interpret and parse.

Further help
~~~~~~~~~~~~

If you require further assistance in migrating to ``InterProScan`` 6 or have any question, or
which for additional features to be implemented in ``InterProScan`` 6 please `contact us <Feedback.html>`__.
