==============================
Advanced Configuration Options
==============================

The basic operation of ``InterProScan`` is configured via the command line. You can find a list of 
the command line options in the `command-line arguments <HowToRun.html#command-line-arguments>`_ section of the docs.

Advanced and utility parameters are configured via the ``InterProScan`` ``nextflow.config`` file.

When ``InterProScan`` is launched, Nextflow looks for a ``nextflow.config`` configuration file. We 
have placed the ``nextflow.config`` in the root of the ``InterProScan`` code base. If you wish 
to use a configuration file that is located else where se the ``-c`` flag and provide the path 
to the configuration file (note the single dash as this is a Nextflow flag).

.. code-block:: bash

    nextflow run interproscan.nf \
        --input <fasta file> \
        -c <path to config file> \
        -profile <profiles>


This page provides an overview of some of the available configuration options in the ``InterProScan``
6 ``nextflow.config`` file.

Many of the terms in the ``nextflow.config`` file set the default values for the command-line flags.
Listed below are the keys that are not configurable via the command-line, and which you may wish 
to alter depending upon your requirements.

.. WARNING::
    We do not recommend altering the values of the keys that are not listed below.

System specific profiles
^^^^^^^^^^^^^^^^^^^^^^^^

Owing to differences in permissions, architectures, and preferences, you may need 
to create a new profile that is bespoke to your system. 

Each profile is stored as a separate configuration file within ``utilities/profiles``, and 
are listed in the ``nextflow.config`` file. 

For more information on creating adding new Nextflow Profiles to ``InterProScan`` please see 
the `"Profiles" documentation <Profiles.rst>`_.

Default applications
^^^^^^^^^^^^^^^^^^^^

The value of the ``applications`` key is a comma separated list of all applications (or member 
databases) that are run as default when the ``--applications`` flag is not used. For example:

.. code-block:: groovy

    applications = 'AntiFam,CDD,NCBIfam,Panther,SFLD'

You may wish to alter the list of default applications, for example, if you want to include any of the 
licensed software in an analysis by default.

Configuring the prediction of open reading frames
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

When nucleic acid sequences are submitted to ``InterProScan``, you can 
configure the prediction of the ORFs by ``esl-translate`` by updating the 
relevant ``translate`` parameters in the ``InterProScan`` ``nextflow.config`` file:

.. code-block:: groovy

    translate {
        strand = 'both'
        methionine = false
        min_len = 20
        genetic_code = 1
    }

**Strand:** The DNA strand(s) to be translated: ``'both'``, ``'plus'``, or ``'minus'``

**Methionine:** Whether the predicted ORFs must start with a methionine (M): ``false`` or ``true``

**Min_len (minimum length):** The minimum length a predicted ORF can be. (Any integer)

**Genetic code:** The number of the genetic code to use. You can find a list of the genetic codes 
in the :ref: `How to analyse Nucleic sequences` documentation.
