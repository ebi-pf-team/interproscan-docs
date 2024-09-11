Running InterProScan
====================

``InterProScan`` is run using the following command (presuming the terminal is pointed at the 
directory containing the ``InterProScan`` code base and InterPro release data):

.. code-block:: bash

    nextflow run <path to the interproscan.nf> \
        -profile <executorprofile, containerRuntime profile> \
        --input <path to fasta file>

.. ATTENTION::
    All ``InterProScan`` custom flags, such as ``--input``, use **double** dashes.  
    All Nextflow flags, such as ``-profile``, use a **single** dash.

The ``--help`` flag is used to print the help message,
which lists all available flags/options for ``InterProScan``:

.. code-block:: bash

    $ nextflow run interproscan.nf --help

    Nextflow 24.04.4 is available - Please consider updating your version to it

    N E X T F L O W   ~  version 24.04.2

    Launching `interproscan.nf` [astonishing_leavitt] DSL2 - revision: 8d1c18b8be

    Usage example:
        nextflow run interproscan.nf --input <path to fasta file>

    Params options:
        --applications <ANALYSES>          Optional, comma separated - without spaces - list of analysis methods (i.e. member databases/applications).
                                            If this option is not set, ALL analyses will be run.
        --datadir <DATA-DIR-PATH>          Optinal. Path to the data directory if data is not located in the 
                                            InterProScan project directory.
        --disable_precalc                  Optional. Disables use of the precalculated match lookup service.
                                            All match calculations will be run locally.
        --formats <FORMATS> Optional, comma separated - without spaces - list of output formats.
        --goterms Optional. Include GO terms in the output.
        --help                             Optional, display help information
        --input <INPUT-FILE-PATH>          [REQUIRED] Path to fasta file that should be loaded on Master startup.
        --nucleic                          Optional. Input comprises nucleic acid sequences.
        --outdir <OUTPUT-DIR-PATH>         Optional. Path to the output directory.
                                            If this option is not set, the output will be write to the current working dir.
                                            All output filenames are prefixed with the input FASTA filename.
        --pathways Optional. Include pathway information in the output.
        --signalp_mode Optional. Set which SignalP/SignalP_EUK prediction models are used. Models may have to be installed. Accepted: 'fast', 'slow', 'slow-sequential'. Default: 'fast'.
        --version                          Print the version of InterProScan.

.. TIP::
    You can find a complete list of all ``InterProScan`` flags in the `Command-line arguments`_ 
    section below.

Default Operation
~~~~~~~~~~~~~~~~~

By default, ``InterProScan`` compares the **protein sequences** provided in an input FASTA file
against all member databases in the InterPro consortium (you can find a list of the members
on the `InterPro website <https://www.ebi.ac.uk/interpro/about/consortium/>`_).

``InterProScan`` also uses the InterPro Match Lookup Service (MLS) to retrieve 
pre-calculated matches that are already present in InterPro (which includes more 
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

The built-in executor profiles are ``local`` and ``slurm``.  
The built-in container runtime profiles are ``docker``, ``singularity``, and ``apptainer``.  

.. IMPORTANT::
    ``-profile`` has a single dash because this is a Nextflow argument. ``--input`` has a double dash because this is a ``InterProScan`` argument.

For example, to run ``InterProScan`` to analyse the protein sequences in 
the example input file ``mini_test.fasta`` using Docker locally, you could use the following command: 

.. code-block:: bash

    nextflow run interproscan.nf \
        -profile docker,local \
        --input utilities/test_files/mini_test.fasta
        

.. TIP::
    When running ``InterProScan6`` locally it is not essential to specify the local profile, 
    although it is recommended to improve resource management and retrying jobs that fail.

.. NOTE::
    To analyse nucleic acid sequences please see the 
    `"How to Analyse Nucleic Sequences" documentation <HowToNucleic.html>`_

Command-line arguments
~~~~~~~~~~~~~~~~~~~~~~

Here is a brief summary of each of the command-line arguments. Each argument is covered 
in more detail in the sections below.

Required arguments
------------------

``-profile`` - Define the ``InterProScan`` profile(s) to use.

The built-in executor profiles are ``local``, ``lsf``, and ``slurm``.  
The built-in container runtime profiles are ``docker``, ``singularity``, and ``apptainer``.  

``--input`` - Path to an input FASTA file of protein or nucleic acids sequences.

.. WARNING:: 
    The input FASTA file must only protein sequences or only nucleic acid sequences.

Optional arguments
------------------

Configuring the analysis
^^^^^^^^^^^^^^^^^^^^^^^^

``--applications`` - [String] Define a subset of applications (member databases) to be used in the analysis, defined as a 
comma separated list, e.g. ``--applications sfld,panther,ncbifam``. Case insensitive.

``--disable_precalc`` - [Boolean] Configures ``InterProScan`` to not retrieve precalculated matches 
from the InterPro Match-Lookup Service (MLS). 
``InterProScan`` will, therefore, run the analyses on all sequences provided in the input FASTA file.

``--nucleic`` - [Boolean] Indicates to ``InterProScan`` that the input file contains nucleic acid
sequences. ``InterProScan`` will predict all potential open reading frames in each nucleic acid 
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
These mappings are based on the matched manually curated InterPro entries.

``--pathways`` - [Boolean] Configures ``InterProScan`` to include mappings from the signature matches to 
the pathway information from the corresponding InterPro entries. These pathway data are from the 
MetaCyc and Reactome pathway databases.

.. TIP::
    More information on choosing 
    the output file formats and including mapped Gene Ontology (GO) terms and Pathway data 
    in the output files can be found in the `Customising the output`_ section below.

Configuring SignalP
^^^^^^^^^^^^^^^^^^^

``--signalp_mode`` - Set which ``SignalP`` / ``SignalP_EUK`` prediction models are used. Models may have 
to be installed manually. Accepted: ``fast``, ``slow``, ``slow-sequential``

Use the application name ``SignalP`` to run ``SignalP`` using all available models.

Use the application name ``SignalP_EUK`` to run ``SignalP`` with the ``--organism eukaryote`` flag 
set. As stated in the `SignalP README <https://github.com/chenxi-zhang-art/signalP>`_:

> Specifying the eukarya method of SignalP6 (SignalP_EUK) triggers post-processing of the SP predictions by SignalP6 to prevent spurious results (only predicts type Sec/SPI).

Utilities
^^^^^^^^^

``--datadir`` - Path to the data directory. By default, ``InterProScan`` looks for a ``data`` directory within 
the ``InterProScan`` project directory.

``--citations`` - [Boolean] Display the citations for ``InterProScan``, all third party tools and 
all members of the InterPro consortium. Analysis does not run.


``--version`` - [Boolean] Display the version number of the InterProScan software you are running. 
Analysis does not run.


Selecting member databases
~~~~~~~~~~~~~~~~~~~~~~~~~~

By default, ``InterProScan`` compares the protein sequences provided in an input FASTA file
against all member databases in the InterPro consortium. You can use the ``--applications`` 
flag to define a subset of member databases as a comma separated list, for example:

.. code-block:: bash

    nextflow run interproscan.nf \
        -profile <profiles> \
        --input <path to input fasta file> \
        --applications "antifam,sfld"

For example, to analyse the protein sequences in the example input fasta file ``utilities/test_files/best_to_test.fasta``
against only the AntiFam and NCBIFam member databases, using an Apptainer image
(`see the Alternative Container docs <AlternativeContainers.html>`_ on how to build an Apptainer image),
you could use:

.. code-block:: bash

    nextflow run interproscan.nf \
        -profile apptainer
        --input utilities/test_files/best_to_test.fasta \
        --applications "antifam,ncbifam"

.. NOTE::
    The member database (or 'applications') names are case insensitive,  both 
    ``ANTIFAM,NCBIFAM`` and ``AntiFam,NCBIfam`` are acceptable.

Below is a list of the currently supported member databases/applications:

* AntiFam
* CDD
* Coils
* FunFam
* Gene3D
* HAMAP
* DeepTMHMM*
* MobiDB*
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
* SignalP*
* SignalP_EUK*
* SMART
* SUPERFAMILY
  
\* - Licensed software (see the :ref:`Installing Licensed Applications` documentation).

Use the application name ``SignalP`` to run ``SignalP`` using all available models,
and application name ``SignalP_EUK`` to run ``SignalP`` with the ``--organism eukaryote`` flag 
set. As stated in the `SignalP README <https://github.com/chenxi-zhang-art/signalP>`_:

> Specifying the eukarya method of SignalP6 (SignalP_EUK) triggers post-processing of the SP predictions by SignalP6 to prevent spurious results (only predicts type Sec/SPI).

Disable looking for precalculated matches in InterPro
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

With the aim to reduce the runtime and computational demand, 
``InterProScan``  uses the InterPro Match Lookup Service (MLS) to retrieve pre-calculated matches,
running the analyses only sequences were a precalculated match is not retrieved. 
In order to use the InterPro MLS your system will need to have external 
access to http://www.ebi.ac.uk.

If you do not wish or are unable to use the InterPro MLS, you can disable looking for 
precalculated matches by including the ``--disable_precalc`` flag in your ``InterProScan``
command:

.. code-block:: bash

    nextflow run interproscan.nf \
        -profile <profile>
        --input <path to input fasta file> \
        --disable_precalc

For example, to analyse the protein sequences in the example input fasta 
file ``utilities/test_files/mini_test.fasta``
against only Panther and SFLD, without retrieving precalculated matches from InterPro, and
using Docker as the container runtime on your local system, you could run:

.. code-block:: bash

    nextflow run interproscan.nf --input utilities/test_files/best_to_test.fasta \
        -profile docker,local \
        --applications panther,sfld \
        --disable_precalc

.. NOTE::
    The order the flags (e.g. ``--input``, ``--applications``, ``-profile``) does **not** matter.

Running on a cluster
~~~~~~~~~~~~~~~~~~~~

The ``InterProScan`` 6 installation does not need to be reconfigured to run on a cluster, but 
you may need to `build alternative containers <AlternativeContainers.html>`_ if 
Docker is not supported on your system.

At the moment, ``InterProScan`` provides only built-in support for the SLURM and LSF schedulers.

To run ``InterProScan`` using the SLURM scheduler use the provided ``slurm`` profile and the 
appropriate container run time  in the ``-profile`` option:

.. code-block:: bash

    nextflow run interproscan.nf \
        -profile slurm,<containerRuntime> \
        --input <input fasta> 

For example, to analyse protein sequences in the example input fasta file ``utilities/test_files/best_to_test.fasta``
against only the Gene3D and FunFam member databases, using a Singularity image,
you could use:

.. code-block:: bash

    nextflow run interproscan.nf \
        -profile slurm,singularity \
        --input utilities/test_files/best_to_test.fasta \
        --applications "funfam,gene3d"

.. WARNING::

    It is never good practise to launch long running jobs in a login/head node
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
^^^^^^^^^^^^^^^^^^^^^^^^^^

By default ``InterProScan`` writes the output files to the current working directory.

Use the ``--outdir`` flag to provide a path to the desired output directory. This can be a relative 
or absolute path.

``InterProScan`` will build all necessary parent directories for the output files.

The output filenames are always prefixed with the input FASTA file name.

.. WARNING::

    ``InterProScan`` will overwrite any existing output files with the same file path in
    an already existing output directory.

Formats
^^^^^^^

You can chose which output file formats that any results are written to using the ``--formats`` option
and providing a comma separate list. The supported file types are ``XML``, ``JSON`` and ``TSV``.

For example, running ``InterProScan`` to analyses example input file ``best_to_test.fasta``, using 
all member databases on a SLURM cluster with Singularity, generating only ``JSON`` and ``TSV`` files:

.. code-block:: bash

    nextflow run interproscan.nf \
        -profile slurm,singularity \
        --input utilities/test_files/best_to_test.fasta \
        --format json,tsv

You can find a description of the output file schemas in `"Output formats" documentation <OutputFormats.html>`_. 

GO terms and pathways
^^^^^^^^^^^^^^^^^^^^^

Gene Ontology (GO) terms are standardised vocabulary terms used to describe the biological 
functions, processes, and cellular locations of genes and gene products (such as proteins) 
across different species.

``InterProScan`` can be configured to map additional GO terms and Pathways data 
from the InterPro database onto the calculated and pre-calculated matches by including the ``--goterms`` and 
``--pathways`` flags respectively.

For example, to run ``InterProScan`` on the example input file ``best_to_test.fasta``, using 
only the CDD and Coils member databases, running locally with Docker, and including additional 
GO terms and pathways mapping in the results:

.. code-block:: bash

    nextflow run interproscan.nf \
        --input utilities/test_files/best_to_test.fasta \
        -profile docker,local \
        --pathways \
        --goterms

.. NOTE::
    The GO terms and Pathways data are downloaded at the same time as the member database data
    during the initially ``InterProScan`` installation. Therefore, internet access is 
    **not** required in order to include these data in the final resutls.

Moving the work (temporary) directory
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Nextflow stores all temporary or working files inside a ``work`` directory in the current working 
directory.

Use the ``-w`` / ``-work-dir`` flag to define the path of the directory where intermediate 
result files are stored (note the **single** dash as this is a Nextflow flag).

.. TIP::

    You can see all Nextflow run time flags by running ``nextflow help run``.

Understanding the terminal output
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The terminal output of ``InterProScan`` allows you to track the progress of the pipeline in 
realtime, as well as providing information about the versions of software and applications used 
in the analysis.

The first section of the ``InterProScan`` terminal output includes the version of Nextflow, and the 
name of the container created by Nextflow from the ``interproscan6`` container image during the run. 
In the extract of the terminal output below, the Nextflow version is ``24.04.02`` and the Docker 
container is called ``stupefied_dalembert``. This information is immediately followed by the 
citations for ``InterProScan`` and ``InterPro``.

.. code-block:: bash

    $ nextflow run interproscan.nf \
        -profile docker \
        --input utilities/test_files/best_to_test.fasta \
        --formats json,tsv \
        --applications antifam,ncbifam,gene3d,funfam,sfld

    N E X T F L O W   ~  version 24.04.2

    Launching `interproscan.nf` [stupefied_dalembert] DSL2 - revision: ec35ea4e85


    If you use InterProScan in your work please cite:

    InterProScan:
    > Jones P, Binns D, Chang HY, Fraser M, Li W, McAnulla C, McWilliam H,
    Maslen J, Mitchell A, Nuka G, Pesseat S, Quinn AF, Sangrador-Vegas A,
    Scheremetjew M, Yong SY, Lopez R, Hunter S.
    InterProScan 5: genome-scale protein function classification.
    Bioinformatics. 2014 May 1;30(9):1236-40. doi: 10.1093/bioinformatics/btu031.
    Epub 2014 Jan 21. PMID: 24451626; PMCID: PMC3998142.

    InterPro:
    > Paysan-Lafosse T, Blum M, Chuguransky S, Grego T, Pinto BL, Salazar GA, Bileschi ML,
    Bork P, Bridge A, Colwell L, Gough J, Haft DH, Letunić I, Marchler-Bauer A, Mi H,
    Natale DA, Orengo CA, Pandurangan AP, Rivoire C, Sigrist CJA, Sillitoe I, Thanki N,
    Thomas PD, Tosatto SCE, Wu CH, Bateman A.
    InterPro in 2022. Nucleic Acids Res. 2023 Jan 6;51(D1):D418-D427.
    doi: 10.1093/nar/gkac993. PMID: 36350672; PMCID: PMC9825450.

After this, ``InterProScan`` prints to the terminal the number of sequences to be analysed.

If a FASTA file containing protein sequences is submitted, the number of sequenes to be analysed 
will match the number of sequences in the input FASTA file. However, if the input 
FASTA file contains nucleotide sequences, the number of sequences to be analysed will be 
far greater owing to potential for multiple open reading frames to be predicted from a single 
nucleic acid sequence.

.. code-block:: bash

    $ nextflow run interproscan.nf \
        -profile docker \
        --input utilities/test_files/best_to_test.fasta \
        --formats json,tsv \
        --applications antifam,ncbifam,gene3d,funfam,sfld
    ...
    Number of sequences to analyse: 253

Next, Nextflow tracks the progress of the various processes it spawns in a tablular format.

The first column (e.g. ``[14/d20fa1]``) identifies the subdirectory within the ``work/`` directory
(created by Nextflow) where the process is running. This directory will include input and output files
for the process.

The second column (e.g. ``process > SEQUENCE_ANALYSIS:GENE3D_HMMER_PARSER``) identifies the type of 
task (e.g. ``process``), followed by the associated subworkflow (e.g. ``SEQUENCE_ANALYSIS``), which 
is separated from the module name by a semi colon (e.g. ``:GENE3D_HMMER_PARSER``). The number 
in parenthesises identifies the total number of instances of that process that have been spawned.

The third column (e.g. ``[100%] 3 of 3 ✔``) indicates the percentage of the currently spawned instances 
of the process that have been completed. Additionally, this column lists the total number and 
number of completed tasks. As the pipeline runs, the number of instances may increase.

The extract from the terminal output below shows the progress during an ``InterProScan`` run:

.. code-block:: bash

    $ nextflow run interproscan.nf \
        -profile docker \
        --input utilities/test_files/best_to_test.fasta \
        --formats json,tsv \
        --applications antifam,ncbifam,gene3d,funfam,sfld
    ...
    executor >  local (21)
    [14/d20fa1] process > PARSE_SEQUENCE (1)                              [100%] 3 of 3 ✔
    [8d/5dca98] process > SEQUENCE_PRECALC:LOOKUP_CHECK (3)               [100%] 3 of 3 ✔
    [ba/0904b4] process > SEQUENCE_PRECALC:LOOKUP_MATCHES (3)             [100%] 3 of 3 ✔
    [8f/e10375] process > SEQUENCE_PRECALC:LOOKUP_NO_MATCHES (3)          [100%] 3 of 3 ✔
    [0d/124250] process > SEQUENCE_ANALYSIS:GENERIC_HMMER_RUNNER (2)      [100%] 2 of 2 ✔
    [d4/497d7a] process > SEQUENCE_ANALYSIS:GENERIC_HMMER_PARSER (2)      [100%] 2 of 2 ✔
    [36/e044b6] process > SEQUENCE_ANALYSIS:GENE3D_HMMER_RUNNER (1)       [  0%] 0 of 1
    [-        ] process > SEQUENCE_ANALYSIS:GENE3D_HMMER_PARSER           -
    [-        ] process > SEQUENCE_ANALYSIS:GENE3D_CATH_RESEOLVE_HITS     -
    [-        ] process > SEQUENCE_ANALYSIS:GENE3D_ADD_CATH_SUPERFAMILIES -
    [-        ] process > SEQUENCE_ANALYSIS:GENE3D_FILTER_MATCHES         -
    [-        ] process > SEQUENCE_ANALYSIS:FUNFAM_HMMER_RUNNER           -
    [-        ] process > SEQUENCE_ANALYSIS:FUNFAM_HMMER_PARSER           -
    [-        ] process > SEQUENCE_ANALYSIS:FUNFAM_CATH_RESEOLVE_HITS     -
    [-        ] process > SEQUENCE_ANALYSIS:FUNFAM_FILTER_MATCHES         -
    [-        ] process > SEQUENCE_ANALYSIS:PANTHER_HMMER_RUNNER          -
    [-        ] process > SEQUENCE_ANALYSIS:PANTHER_HMMER_PARSER          -
    [-        ] process > SEQUENCE_ANALYSIS:PANTHER_POST_PROCESSER        -
    [-        ] process > SEQUENCE_ANALYSIS:PANTHER_FILTER_MATCHES        -
    [4d/5ca554] process > SEQUENCE_ANALYSIS:SFLD_HMMER_RUNNER (1)         [ 50%] 1 of 2
    [2e/705b8d] process > SEQUENCE_ANALYSIS:SFLD_HMMER_PARSER (1)         [100%] 1 of 1 ✔
    [21/a1386b] process > SEQUENCE_ANALYSIS:SFLD_POST_PROCESSER (1)       [100%] 1 of 1 ✔
    [73/d09d2f] process > SEQUENCE_ANALYSIS:SFLD_FILTER_MATCHES (1)       [100%] 1 of 1 ✔
    [-        ] process > SEQUENCE_ANALYSIS:CDD_RUNNER                    -
    [-        ] process > SEQUENCE_ANALYSIS:CDD_POSTPROCESS               -
    [-        ] process > SEQUENCE_ANALYSIS:CDD_PARSER                    -
    [-        ] process > SEQUENCE_ANALYSIS:SIGNALP_RUNNER                -
    [-        ] process > SEQUENCE_ANALYSIS:SIGNALP_PARSER                -
    [-        ] process > AGGREGATE_RESULTS                               -
    [-        ] process > XREFS:ENTRIES                                   -
    [-        ] process > WRITE_RESULTS                                   -
    
.. NOTE::
    Although ``InterProScan`` takes in a single FASTA file as input to improve the computing 
    efficiency with the aim to reduce the total run time, ``InterProScan``, splits 
    input files into smaller batches.  Each of these batches are processed by all specified 
    applications. Thus, a single process may run multiple times, one for each batch.

``InterProScan`` displays the release version of each member database and application 
that was used in the analysis, and whether the precalculated matches were retrieved from InterPro 
via the InterPro Match Lookup Service (MLS).

.. code-block:: bash

    $ nextflow run interproscan.nf \
        -profile docker \
        --input utilities/test_files/best_to_test.fasta \
        --formats json,tsv \
        --applications antifam,ncbifam,gene3d,funfam,sfld
    ...
    Using precalculated match lookup service
    Running sequence analysis
    Running antifam version 7.0
    Running ncbifam version 14.0
    Running gene3d version 4.3.0
    Running funfam version 4.3.0
    Running sfld version 4
