====================
Running InterProScan
====================

``InterProScan`` can be run using the following command:

.. code-block:: bash

    nextflow run ebi-pf-team/interproscan6 \
          -profile <ContainerRunTime:docker/singularity/apptainer,Executor:lsf/slurm> \
          --input <FASTA> \
          --datadir <DATADIR>

If ``InterProScan`` was `installed from source <<HowToInstall.html>`__, please use the following 
command:

.. code-block:: bash

    nextflow run <path to the interproscan main.nf> \
          -profile <ContainerRunTime:docker/singularity/apptainer,Executor:lsf/slurm> \
          --input <FASTA> \
          --datadir <DATADIR>

.. ATTENTION::

    All ``InterProScan`` flags, such as ``--input``, use **double** dashes.
    All Nextflow flags, such as ``-profile``, use a **single** dash.

For the rest of this page we will use the command ``nextflow run ebi-pf-team/interproscan6``.

The Help Message
~~~~~~~~~~~~~~~~

Use the ``--help`` flag to print the help message,
which lists most available flags:

.. code-block:: bash

    $ nextflow run main.nf --help
     N E X T F L O W   ~  version 25.04.6
    Launching `main.nf` [condescending_meninsky] DSL2 - revision: 07d395ddd2

    # InterProScan6 6.0.0-alpha
    # Genome-scale protein function classification

    Usage: nextflow run ebi-pf-team/interproscan6 -profile <PROFILE> --input <FASTA> --datadir <DATADIR>

    Mandatory parameters:
      -profile <PROFILE>: use this parameter to choose a configuration profile.
      --input <FASTA>                       : path to FASTA file of sequences to be analysed.
      --datadir <DATA-DIR>                  : path to data directory.
    ...

You can find a complete list of all ``InterProScan`` flags in the `Command-line arguments`_ section below.

Default Operation
~~~~~~~~~~~~~~~~~

By default ``InterProScan``:

* Runs **locally** on **bare metal**
* Uses the **latest InterPro** release
* Retrieves pre-calculated matches from the `InterPro Matches API <https://www.ebi.ac.uk/interpro/matches/api>`__
* Analyses **protein sequences** that are not in the InterPro Matches API against **all** activated applications
* Produces the output files: ``GFF3``, ``JSON``, ``JSON-line``, ``TSV`` and ``XML``

Command-line arguments
~~~~~~~~~~~~~~~~~~~~~~

Required arguments
------------------

The only **required** arguments to run ``InterProScan`` are:

* ``--input`` to define the path to the input FASTA file.
* ``--datadir`` to define the path to the InterPro data directory.

The data directory can be pre-populated, or ``InterProScan`` will automatically download and set 
up the necessary data files in the data directory specified.

To run ``InterProScan`` using containers or on a cluster or the cloud you will also need to 
use the ``-profile`` flag to define the container runtime and executor to use.

* The built-in cluster profiles are ``slurm`` and ``lsf`` (default: ``local``).
* The built-in container runtime profiles are ``docker``, ``singularity``, and ``apptainer`` (default: runs on bare metal).

For example, to run ``InterProScan`` to analyse the protein sequences using Docker locally (the default executor):

.. code-block:: bash

    nextflow run ebi-pf-team/interproscan6 \
        -profile docker \
        --input tests/data/test_prot.fa \
        --datadir data

To analyse nucleic acid sequences please see the
`"How to Analyse Nucleic Sequences" documentation <HowToNucleic.html>`_

.. NOTE::
    The ``--datadir``` flag is not needed when only running applications that do not require additional data files, this includes:
    ``mobidblite``, ``coils``, ``TMbed`` and the licensed software ``DeepTMHMM``, ``Phobius``, ``SignalP``.

Optional arguments
------------------

Configuring the analysis
^^^^^^^^^^^^^^^^^^^^^^^^

``--interpro`` - [String] Specify the InterPro data version at run time, e.g. ``--interpro 107.0``. Defaults to the latest.

``--applications`` - [String] Comma separated list of of applications (member databases) to be used 
in the analysis, e.g. ``--applications sfld,panther,gene3d``. Case insensitive.

``--skip-applications`` - [String] Comma separated list of applications (member databases) to be skipped. 
E.g. ``--skip-applications sfld,panther,gene3d``. Case insensitive.

``--run-ml`` - [Boolean] Include machine learning based applications in the analysis, e.g. 
``InterPro-N``, ``SignalP``, ``DeepTMHMM``, ``TMbed``. Default false. See 
`"Installation Licensed Applications" <HowToInstallLicensedApps.html>`__ for information on installing these applications.

``--no-matches-api`` - [Boolean] do **not** retrieve precalculated matches from the InterPro Matches API
(connecting to the Matches API requires an internet connection), and run the analyses against all submitted sequences.

``--nucleic`` - [Boolean] Analyse and input FASTA file of nucleic acid sequences. You can find out more in the
`"How to Analyse Nucleic Sequences" documentation <HowToNucleic.html>`__.

For example, to analyse protein sequences against only Panther, SFLD and the machine learning based
applications, without retrieving precalculated matches from InterPro, and using Docker 
on your local system with GPU acceleration for applicable applications, you could run:

.. code-block:: bash

    nextflow run ebi-pf-team/interproscan6 \
        -profile docker \
        --input tests/data/test_prot.fa \
        --datadir data \
        --applications panther,sfld \
        --run-ml \
        --use-gpu \
        --no-matches-api

Configuring the output data
^^^^^^^^^^^^^^^^^^^^^^^^^^^

``--outdir`` - [String] Path to the output directory. Default: current working directory. 
This can be an absolute or relative path. ``InterProScan`` will build the
output directory and all necessary parent directories.

``--outprefix`` - [String] Base name for output files, without directory. 
The extension is automatically added to the file. This only affects the filename, not its location. 
It must not contain slashes, path components or spaces. Default: input fasta filename.

.. WARNING::

    ``InterProScan`` will overwrite any existing output files with the same file path.

``--formats`` - [String] Comma separated listed of output file formats. Default: all. Supported
formats: ``GFF3``, ``JSON``, ``JSONL``, ``TSV``, and ``XML``. Case insensitive. Default: 
You can find descriptions of the output file schemas in `"Output formats" documentation <OutputFormats.html>`__.

``--goterms`` - [Boolean] Configures ``InterProScan`` to include Gene Ontology (GO) terms in the output files. 
These mappings are based on the manually curated InterPro entries.

``--pathways`` - [Boolean] Configures ``InterProScan`` to include mappings from the signature matches to 
the pathway information from the corresponding InterPro entries. These pathway data are from the 
MetaCyc and Reactome pathway databases.

.. NOTE::
    The GO terms and Pathways data are downloaded at the same time as the member database data
    during the initially ``InterProScan`` installation. Therefore, internet access is
    **not** required in order to include these data in the final results.

For example, running ``InterProScan`` to analyses protein sequences using
all member databases on a SLURM cluster with Singularity, generating only ``JSON`` and
``TSV`` files that include goterms and pathway annotations, and writing the results to
the output dir ``my_results/analysis_57``:

.. code-block:: bash

    nextflow run ebi-pf-team/interproscan6 \
        -profile slurm,singularity \
        --input tests/data/test_prot.fa \
        --datadir interpro-104.0/ \
        --format json,tsv \
        --outdir my_results/analysis_57 \
        --goterms \
        --pathways

Analysing large datasets
^^^^^^^^^^^^^^^^^^^^^^^^

``--batch-size`` - [Integer] Number of sequences per batch. Default 5000.

``--sub-batch-size`` - [Integer] Number of sequences per sub-batch. Default 1000.

For large datasets, you may be able to improve performance by increasing the batch size and sub-batch size.
However, this will also increase the memory requirements of the analysis. We recommend using the 
provided ``bulk`` profile which increases the batch size, sub-batch size and resource allocations.

.. code-block:: bash

    nextflow run ebi-pf-team/interproscan6 \
        -profile sinularity,slurm,bulk \
        --input <FASTA> \
        --datadir <DATADIR>

Configuring resources
^^^^^^^^^^^^^^^^^^^^^

``--maxWorkers`` - [Integer] The maximum number of jobs running in parallel at any given moment. Default equal to the number of available CPUs minus one.

``--cpus`` - [Integer] Number of CPUs assigned to each analysis job (e.g. HMMER). Default 1.

``--use-gpu`` - [Boolean] Enable GPU acceleration for all supported applications (DeepTMHMM, SignalP, InterPro-N and TMbed). Default false.

To specify GPU accelleration for individual applications please see the 
`"Installing Licensed Applications" <HowToInstallLicensedApps.html>`__ documentation.

Moving the work (temporary) directory
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Nextflow stores all temporary files inside a ``work`` directory in the current working
directory. Use the ``-w`` / ``-work-dir`` flag to define the path of the directory where intermediate
files are stored (note the **single** dash as this is a Nextflow flag).

.. TIP::

    You can see all Nextflow run time flags by running ``nextflow help run``.

Understanding the terminal output
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The terminal output of ``InterProScan`` allows you to track the progress of the pipeline in 
realtime.

.. code-block:: bash

    $ nextflow run ebi-pf-team/interproscan6 \
         -profile docker
         --input tests/data/test_prot.fa \
         --datadir data \
         --applications ncbifam,antifam

     N E X T F L O W   ~  version 24.10.0

    Launching `ebi-pf-team/interproscan6` [amazing_dalembert] DSL2 - revision: bafba8847a

    # InterProScan6 6.0.0-alpha
    # Genome-scale protein function classification

    executor >  local (10)
    [83/6d3f04] process > PREPARE_PROTEIN_SEQUENCES (1)    [100%] 1 of 1 ✔
    [ad/09104a] process > SCAN_SEQUENCES:RUN_ANTIFAM (1)   [100%] 1 of 1 ✔
    [6b/1225f5] process > SCAN_SEQUENCES:PARSE_ANTIFAM (1) [100%] 1 of 1 ✔
    [5c/a237d2] process > SCAN_SEQUENCES:RUN_NCBIFAM (1)   [100%] 1 of 1 ✔
    [a9/1bfe8d] process > SCAN_SEQUENCES:PARSE_NCBIFAM (1) [100%] 1 of 1 ✔
    [95/803c6d] process > XREFS (1)                        [100%] 1 of 1 ✔
    [eb/b2f519] process > AGGREGATE_SEQS_MATCHES (1)       [100%] 1 of 1 ✔
    [61/24891f] process > AGGREGATE_ALL_MATCHES            [100%] 1 of 1 ✔
    [23/652963] process > WRITE_TSV_OUTPUT                 [100%] 1 of 1 ✔
    [d6/53c9e3] process > WRITE_XML_OUTPUT                 [100%] 1 of 1 ✔

1. The first column (e.g. ``[83/6d3f04]``) identifies the work subdirectory where the process is running, and where the output and log files for the process can be found (useful for trouble shooting).
2. The second column identifies the type of task (e.g. ``process``), and the name of the task (e.g. ``SCAN_SEQUENCES:RUN_ANTIFAM```)
3. The third column (e.g. ``[100%] 1 of 1 ✔``) indicates the percentage and number of the currently spawned instances of a given process that have been completed.

Although ``InterProScan`` takes in a single FASTA file as input to improve the computing
efficiency, ``InterProScan`` may split the FASTA file into smaller batches.  Each of these batch
is analysed by all specified applications. Thus, a single process may run multiple times, 
one for each batch or even sub-batch depending on the application.
