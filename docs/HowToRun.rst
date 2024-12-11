====================
Running InterProScan
====================

``InterProScan`` can be run using the following command:

.. code-block:: bash

    nextflow run ebi-pf-team/interproscan6 \
          -profile <ContainerRunTime:docker/singularity/apptainer...Executor:local/lsf/slurm> \
          --input <FASTA> \
          --datadir <DATADIR>

If ``InterProScan`` was `installed from source <<HowToInstall.html>`__, please use the following 
command:

.. code-block:: bash

    nextflow run <path to the interproscan main.nf> \
          -profile <ContainerRunTime:docker/singularity/apptainer...Executor:local/lsf/slurm> \
          --input <FASTA> \
          --datadir <DATADIR>

.. ATTENTION::
    All ``InterProScan`` flags, such as ``--input``, use **double** dashes.
    All Nextflow flags, such as ``-profile``, use a **single** dash.

For the rest of this page we will use the command ``nextflow run ebi-pf-team/interproscan6...``.

The Help Message
~~~~~~~~~~~~~~~~

Use the ``--help`` flag to print the help message,
which lists all available flags:

.. code-block:: bash

    $ nextflow run main.nf --help
     N E X T F L O W   ~  version 24.10.0
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

By default, ``InterProScan`` compares the **protein sequences** provided in an input FASTA file
against all member databases (specifically, all databases that are listed in ``conf/applications.config``).

``InterProScan`` uses the InterPro Match Lookup Service (MLS) to retrieve
pre-calculated matches that are already present in InterPro (which constitutes more
than 500 million protein sequences, including all sequences in UniProtKB). The sequence 
analyses are only run for those sequences where a precalculated match is not available, thus
reducing the total run time and computational demand.

The default output file formats for ``InterProScan`` are ``JSON``, ``TSV`` and ``XML``.

Quick Start
~~~~~~~~~~~

The only **required** arguments to run ``InterProScan`` are:

``-profile`` - Used to define the executor and the container runtime used.

``--input`` - Used to define the path to the input file containing the query sequences to be 
analysed in FASTA format.

``--datadir`` - Used to define the path to the downloaded InterPro data directory, containing the member database models.

The built-in executor profiles are ``local``, ``slurm`` and ``lsf``.
The built-in container runtime profiles are ``docker``, ``singularity``, and ``apptainer``.  

For example, to run ``InterProScan`` to analyse the protein sequences using Docker locally:

.. code-block:: bash

    nextflow run ebi-pf-team/interproscan6 \
        -profile docker,local \
        --input tests/data/test_prot.fa \
        --datadir data

.. TIP::
    When running ``InterProScan6`` locally it is not essential to specify the local profile, 
    although it is recommended to improve resource management and retrying jobs that fail.

.. NOTE::
    To analyse nucleic acid sequences please see the 
    `"How to Analyse Nucleic Sequences" documentation <HowToNucleic.html>`_

.. NOTE::
    The ``--datadir``` flag is not needed when only running member databases that do not require additional data files.
    This only applies to ``mobidblite`` and ``coils``` (which do not require additional datafiles) and the
    licensed software (``SignalP``, ``Phobius``, and ``TMHMM```).

Command-line arguments
~~~~~~~~~~~~~~~~~~~~~~

Required arguments
------------------

* ``-profile`` - Define the ``InterProScan`` profile(s) to use.
* ``--input`` - Path to an input FASTA file of protein or nucleic acids sequences.
* ``--datadir`` - Path to the InterPro data directory.

The built-in executor profiles are ``local``, ``lsf``, and ``slurm``.
The built-in container runtime profiles are ``docker``, ``singularity``, and ``apptainer``.

.. WARNING:: 
    The input FASTA file must contain only protein sequences or only nucleic acid sequences.

Optional arguments
------------------

Configuring the analysis
^^^^^^^^^^^^^^^^^^^^^^^^

``--applications`` - [String] Define a set of applications (member databases) to be used in the analysis, defined as a
comma separated list, e.g. ``--applications sfld,panther,gene3d``. Case insensitive.

``--disablePrecalc`` - [Boolean] Configures ``InterProScan`` to not retrieve precalculated matches
from the InterPro Match-Lookup Service (MLS). 
``InterProScan`` will, therefore, run the analyses on all sequences provided in the input FASTA file.

``--nucleic`` - [Boolean] Indicates to ``InterProScan`` that the input file contains nucleic acid
sequences, triggering ``InterProScan`` to predict potential open reading frames in each
sequence using the `easel software suite <https://github.com/EddyRivasLab/easel>`_ from the 
Eddy/Rivas lab group.

.. TIP::
    You can find out more 
    in the  `"How to Analyse Nucleic Sequences" documentation <HowToNucleic.html>`_

Configuring the output data
^^^^^^^^^^^^^^^^^^^^^^^^^^^

``--outdir`` - [String] Define the path to the output directory. By default ``InterProScan`` 
writes to the current working directory. This can be an absolute or relative path. The output
filenames are always prefixed with the input FASTA filename.

``--formats`` - [String] Define the output file formats as a comma separated list. The options 
are ``JSON``, ``TSV``, and ``XML``. E.g. ``--formats tsv,xml``. Case insensitive. Default: 
``JSON,TSV,XML``

``--goterms`` - [Boolean] Configures ``InterProScan`` to include Gene Ontology (GO) terms in the output files. 
These mappings are based on the manually curated InterPro entries.

``--pathways`` - [Boolean] Configures ``InterProScan`` to include mappings from the signature matches to 
the pathway information from the corresponding InterPro entries. These pathway data are from the 
MetaCyc and Reactome pathway databases.

.. TIP::
    More information on choosing 
    the output file formats and including mapped Gene Ontology (GO) terms and Pathway data 
    in the output files can be found in the `Customising the output`_ section below.

Configuring SignalP
^^^^^^^^^^^^^^^^^^^

``--signalpMode`` - Set which ``SignalP_Prok`` / ``SignalP_EUK`` prediction models are used. Models may have
to be installed manually. Accepted: ``fast``, ``slow``, ``slow-sequential``

Use the application name ``SignalP_Prok`` to run ``SignalP`` using all available models.

Use the application name ``SignalP_Euk`` to run ``SignalP`` with the ``--organism eukaryote`` flag
set. As stated in the `SignalP README <https://github.com/chenxi-zhang-art/signalP>`_:

Utilities
^^^^^^^^^

``--citations`` - [Boolean] Display the citations for ``InterProScan``, all third party tools and 
all members of the InterPro consortium. Analysis does not run.

``--version`` - [Boolean] Display the version number of the InterProScan software you are running. 
Analysis does not run.

Selecting member databases
~~~~~~~~~~~~~~~~~~~~~~~~~~

By default, ``InterProScan`` compares the protein sequences provided in an input FASTA file
against all InterPro member databases. Use the ``--applications``
flag to define a set of member databases as a comma separated list, for example:

.. code-block:: bash

    nextflow run ebi-pf-team/interproscan6 \
        -profile <profiles> \
        --input <path to input fasta file> \
        --datadir <interpro data dir> \
        --applications "antifam,sfld"

For example, to analyse protein sequences using only the AntiFam and NCBIFam member databases, and an Apptainer image locally
use:

.. code-block:: bash

    nextflow run ebi-pf-team/interproscan6 \
        -profile apptainer,local \
        --input tests/data/test_prot.fa \
        --datadir interpro-103.0 \
        --applications "antifam,ncbifam"

.. NOTE::
    The member database (or 'applications') names are case **insensitive**,  both
    ``ANTIFAM,NCBIFAM`` and ``AntiFam,NCBIfam`` are acceptable.

Below is a list of the currently supported member databases/applications:

* AntiFam
* CDD
* Coils
* FunFam
* Gene3D
* HAMAP
* DeepTMHMM*
* MobiDB
* NCBIFam
* Panther
* Pfam
* Phobius*
* PIRSF
* PIRSR
* Prints
* Prosite Patterns
* Prosite Profiles
* SFLD
* SignalP_Prok*
* SignalP_EUK*
* SMART
* SUPERFAMILY

\* - Licensed software (see the :ref:`Installing Licensed Applications` documentation).

Disable looking for precalculated matches in InterPro
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

With the aim to reduce the runtime and computational demand, 
``InterProScan``  uses the InterPro Match Lookup Service (MLS) to retrieve pre-calculated matches,
running the analyses only sequences were a precalculated match is not retrieved. 
In order to use the InterPro MLS your system will need to have external 
access to http://www.ebi.ac.uk.

If you do not wish or are unable to use the InterPro MLS, you can disable looking for 
precalculated matches by including the ``--disablePrecalc`` flag in your ``InterProScan``
command:

.. code-block:: bash

    nextflow run ebi-pf-team/interproscan6 \
        -profile <profile> \
        --input <path to input fasta file> \
        --datadir <path to interpro data dir> \
        --disablePrecalc

For example, to analyse protein sequences against only Panther and SFLD, without retrieving precalculated matches
from InterPro, and using Docker as the container runtime on your local system, you could run:

.. code-block:: bash

    nextflow run ebi-pf-team/interproscan6 --input tests/data/test_prot.fa \
        -profile docker,local \
        --input tests/data/test_prot.fa \
        --datadir interpro-103.0 \
        --applications panther,sfld \
        --disablePrecalc

Running on a cluster
~~~~~~~~~~~~~~~~~~~~

To run ``InterProScan``` 6 on a cluster of cloud, use the relevant executor profile for the system. 
See the :ref:`Using Alternative Container Runners` documentation for more information on using alternative container runtimes
to docker.

For example, to run  ``InterProScan`` using the SLURM scheduler:

.. code-block:: bash

    nextflow run ebi-pf-team/interproscan6 \
        -profile slurm,<containerRuntime> \
        --input <input fasta> \
        --datadir <interpro data dir>

At the moment, ``InterProScan`` provides only built-in support for the SLURM and LSF schedulers.
To run ``InterProScan`` using alternative scheduler and cloud systems please refer to the `Profiles page <Profiles.html>`.

For example, to analyse protein sequences against only the Gene3D and FunFam member databases, using an Apptainer image,
you could use:

.. code-block:: bash

    nextflow run ebi-pf-team/interproscan6 \
        -profile slurm,apptainer \
        --input tests/data/test_prot.fa \
        --datadir data \
        --applications funfam,gene3d

.. WARNING::

    ``InterProScan`` is resource intensive. We do not recommend running large analyses on login/head node.
    Run ``InterProScan`` as an interactive job or submit the job via a bash script.

.. IMPORTANT::

    The profiles in ``InterProScan6`` define the time and resource allocations for the analyses. 
    We recommend reviewing the relevant profile configuration files in ``utilities/profiles`` 
    to ensure they met requirements and expected practices of your system. 
    If you are unsure how to deploy Nextflow on your system contact the sysadmin. 
    You can find out more information on the ``InterProScan`` profiles `here <Profiles.html>`. Please 
    refer to this documentation before creating your own profiles.

Customising the output
~~~~~~~~~~~~~~~~~~~~~~

Location of the output dir
--------------------------

* By default ``InterProScan`` writes the output files to the current working directory. 
* Use the ``--outdir`` flag to provide a path to the desired output directory. This can be a relative 
or absolute path.
* ``InterProScan`` will build all necessary parent directories for the output files.
* The output filenames are always prefixed with the input FASTA file name.

.. WARNING::

    ``InterProScan`` will overwrite any existing output files with the same file path.

Formats
-------

You can chose which output file formats that any results are written to using the ``--formats`` option
and providing a comma separate list. The supported file types are ``XML``, ``JSON`` and ``TSV``.

For example, running ``InterProScan`` to analyses protein sequences using 
all member databases on a SLURM cluster with Singularity, generating only ``JSON`` and ``TSV`` files:

.. code-block:: bash

    nextflow run ebi-pf-team/interproscan6 \
        -profile slurm,singularity \
        --input tests/data/test_prot.fa \
        --datadir interpro-103.0/ \
        --format json,tsv

You can find a description of the output file schemas in `"Output formats" documentation <OutputFormats.html>`_. 

GO terms and pathways
---------------------

Gene Ontology (GO) terms are standardised vocabulary terms used to describe the biological 
functions, processes, and cellular locations of genes and gene products (such as proteins) 
across different species.

``InterProScan`` can be configured to map GO terms and Pathways data 
from the InterPro database onto the calculated and pre-calculated matches by including the ``--goterms`` and 
``--pathways`` flags respectively.

For example, to run ``InterProScan`` using only the CDD and Coils member databases, running locally with Docker, 
and including additional GO terms and pathways mapping in the results:

.. code-block:: bash

    nextflow run ebi-pf-team/interproscan6 \
        -profile docker,local \
        --input tests/data/test_prot.fa \
        --datadir interpro-103.0 \
        --pathways \
        --goterms

.. NOTE::
    The GO terms and Pathways data are downloaded at the same time as the member database data
    during the initially ``InterProScan`` installation. Therefore, internet access is 
    **not** required in order to include these data in the final resutls.

Moving the work (temporary) directory
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Nextflow stores all temporary files inside a ``work`` directory in the current working
directory.

Use the ``-w`` / ``-work-dir`` flag to define the path of the directory where intermediate 
files are stored (note the **single** dash as this is a Nextflow flag).

.. TIP::

    You can see all Nextflow run time flags by running ``nextflow help run``.

Understanding the terminal output
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The terminal output of ``InterProScan`` allows you to track the progress of the pipeline in 
realtime.

The first section of the terminal output includes the version of Nextflow and ``InterProScan``, and the
name of the container created by Nextflow from the ``interproscan6`` image during the run.

.. code-block:: bash

    $ nextflow run ebi-pf-team/interproscan6 \
         -profile docker,local
         --input tests/data/test_prot.fa \
         --datadir data \
         --applications ncbifam,antifam

     N E X T F L O W   ~  version 24.10.0

    Launching `ebi-pf-team/interproscan6` [amazing_dalembert] DSL2 - revision: bafba8847a

    # InterProScan6 6.0.0-alpha
    # Genome-scale protein function classification

Next, Nextflow tracks the progress of the various processes it spawns in a tablular format.

.. code-block:: bash

    $ nextflow run ebi-pf-team/interproscan6 \
         -profile docker,local
         --input tests/data/test_prot.fa \
         --datadir data \
         --applications ncbifam,antifam
    ...
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

The first column (e.g. ``[83/6d3f04]``) identifies the subdirectory within the ``work/`` directory
(created by Nextflow) where the process is running, and where the output files
for the process can be found.

The second column (e.g. ``process > SCAN_SEQUENCES:RUN_ANTIFAM``) identifies the type of
task (e.g. ``process``), the associated subworkflow (``e.g. SCAN_SEQUENCES```) and the name
of the task (e.g. ``SCAN_SEQUENCES:RUN_ANTIFAM```).  The number
in parenthesises identifies the total number of spawned instances of that process.

The third column (e.g. ``[100%] 1 of 1 ✔``) indicates the percentage of the currently spawned instances
of the process that have been completed. Additionally, this column lists the total number and 
number of completed tasks.

.. NOTE::
    Although ``InterProScan`` takes in a single FASTA file as input to improve the computing 
    efficiency, ``InterProScan`` may split the FASTA file into smaller batches.  Each of these batch
    is analysed by all specified applications. Thus, a single process may run multiple times, one for each batch.

Then ``InterProScan`` finishes with a summary closing message

.. code-block:: bash

    InterProScan workflow completed successfully: true.
    Any results are located at ./test_prot_noIllegals.fa.ips6.*
    Duration: 1m 16s
    Completed at: 29-Nov-2024 15:27:14
    Duration    : 1m 16s
    CPU hours   : (a few seconds)
    Succeeded   : 10

