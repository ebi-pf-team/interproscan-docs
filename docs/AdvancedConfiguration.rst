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
        -profile <slurm,lsf,local...docker,apptainer,singularity> \
        --input <fasta file> \
        --datadir <interpro data dir> \
        -c <path to config file> \

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
to create a new profile that is bespoke to your system. Each profile is stored as a separate configuration file
within ``utilities/profiles``, and
are listed in the ``nextflow.config`` file. For more information on creating adding new Nextflow
Profiles to ``InterProScan`` please see
the `"Profiles" documentation <Profiles.rst>`_.

Default applications
^^^^^^^^^^^^^^^^^^^^

The default applications (member databases) that are run when the ``--applications`` flag is not used
are defined in configuration file ``conf/applications.config``. Add and remove members from this file to define
the default. Note, if you remove a member from this file you will need to add it back in to be able to run the
member at all.
