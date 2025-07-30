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

.. code-block:: bash

    nextflow run ebi-pf-team/interproscan6 \
        -profile docker\
        --input tests/data/test_prot.fa \
        --datadir <path-to-the-data-dir>

Specify the InterPro version (best practise)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

``InterProScan`` 6 will automatically download the latest version of InterPro. For reproducibility and reproducion
of analyses, use the ``--interpro`` flag to specify InterPro release to use [Default: ``latest``].

.. code-block:: bash

    nextflow run ebi-pf-team/interproscan6 \
        -profile docker\
        --input tests/data/test_prot.fa \
        --datadir data \
        --interpro 104.0

Running InterProScan: Flag changes
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Support only for long-name (double dashed) flags
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

``InterProScan`` 6 only supports long-name (double dashed) flags, therefore, all flags from ``InterProScan``
5 that are supported in ``InterProScan`` 6 must be convereted to their long name.

New flags
^^^^^^^^^

**``-profile`**
Specify the run time profile. Typically the container runtime (e.g. ``docker``), and when not running locally the
executor (e.g. ``slurm``).
*Note this option uses a single dash, not two.*

**``--nucleic``**
``InterProScan`` 6 defaults to analysing protein sequences. To analyse an input FASTA file of nucleotide sequences
use the ``--nucleic`` flag.

Renamed flags
^^^^^^^^^^^^^

**``--disable-precalc`` >> ``--no-matches-api``**
The flag to skip the retrieval of precalculated matches from the InterPro Matches API (``--disable-precalc``)
has been renamed in ``InterProScan`` 6 to ``--no-matches-api``.

**``--output-dir`` >> ``--outdir``**
Specify the output directory using the ``--outdir`` flag. Additionally, unlike ``InterProScan`` 5, ``InterProScan`` 6 will build (including all necessary parent directories) if the
output directory does not already exist.

**``--output-file-base`` >> ``--outprefix``**
Specify the base name for output files, without a directory. The file extension will be added automatically. Whereas in
``InterProScan`` 5 ``--output-file-base`` could be a relative or absolute path, in ``InterProScan`` 6 ``--outprefix``
will only affect the file name, not its location and must not contain slashes, spaces or path components.

**``--excl-applications`` >> ``--skip-annotations``**
Comma separated list of analyses to exclude.

Flags that are no longer supported
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The following ``InterProScan`` 5 flags are not supported in ``InterProScan`` 6:

* ``-cpu,--cpu``
* ``-b,--output-file-base``
* ``-d,--output-dir`` [renamed to ``--outdir``]
* ``-dp,--disable-precalc`` [renamed to ``--no-matches-api``]
* ``-dra,--disable-residue-annot``
* ``-etra,--enable-tsv-residue-annot``
* ``-exclappl,--excl-applications`` [renamed to ``skip-applications``]
* ``-incldepappl,--incl-dep-applications``
* ``-iprlookup,--iprlookup``
* ``-ms,--minsize``
* ``-o,--outfile``
* ``-t,--seqtype`` [``InterProScan`` 6 defaults to analysing protein sequences, use the ``--nucleic`` flag for nucleotide sequences]
* ``-T,--tempdir`` [use the Nextflow ``-work-dir`` flag. Note the single dash]
* ``-verbose,--verbose``
* ``-version,--version`` [the version is always printed out to the terminal when ``InterProScan`` 6 launches]
* ``-vl,--verbose-level``
* ``-vtsv,--output-tsv-version``

Application/Member db name aliases
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The formating of all application names from ``InterProScan`` 5 are accepted in ``InterProScan`` 6.

In addition, ``InterProScan`` 6 includes aliases that allow for inclusion of dashes in the following
application names:

* funfam, cath-funfam, cathfunfam
* gene3d, cath-gene3d, cathgene3d
* mobidblite, mobidb-lite
* prositeprofiles, prosite-profiles
* prositepatterns, prosite-patterns

SignalP renaming
^^^^^^^^^^^^^^^^

SignalP has been upgraded to version 6 in ``InterProScan`` 6, and the application names for ``signalp_gram_negative`` and
``signalp_gram_positive`` have been combined and changed to ``signalp_prok`` for prokaryotic sequences.

TMHMM update
^^^^^^^^^^^^

TMHMM has been upgraded to DeepTMHMM in ``InterProScan`` 6, and the application named to ``deeptmhmm``. ``tmhmm`` is
not supported.

GPU acceleration
^^^^^^^^^^^^^^^^

For significantly reduced compute times, SignalP and DeepTMHMM can be run with GPU acceleration. See the
`Installing Licensed Applications page <InstallingLicensedApps.rst>`__ for more information.

Running on a cluster or cloud
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Unlike ``InterProScan`` 5, ``InterProScan`` 6 does not need to be reconfigured to run on a cluster or a cloud. The same
code base can be used out of the box for running locally, on a cluster, or a cloud.

By default ``InterProScan`` 6 runs in local mode, to run on a cluster or cloud use the ``--profile`` flag.

At the moment, ``InterProScan`` provides only built-in support for the SLURM and LSF schedulers.
See the `profiles page <Profiles.html>`__ documentation for more information on
using alternative system schedulers.

Output files
~~~~~~~~~~~~

For improved parsing of large output files containing many sequences and matches, ``InterProScan`` 6 can produce
a JSON line output file, see the `Output Formats page <OutputFormats.rst>`__ for more information.

The content and structure of the TSV, JSON and GFF3 files are the same between versions 5 and 6, except for two exceptions
in all output files:

* Gene3D and FunFam have been renamed to ``Cath-Gene3D`` and ``Cath-FunFam``.
* The InterPro version and ``InterProScan`` version have been separated.

``InterProScan`` 5:

.. code-block:: json

    }
        "interproscan-version": "5.75-106.0",
        "results": []
    }

``InterProScan`` 6:

.. code-block:: json

    {
        "interproscan-version": "6.0.0-beta",
        "interpro-version": "106.0",
        "results": []
    }


XML Schema changes
^^^^^^^^^^^^^^^^^^

* Root node named from ``protein-matches`` to ``results`` (matching the JSON structure).
* All match nodes and location node names have been renamed from their application/member db based naming to a generic ``match`` and ``location`` nodes.

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
