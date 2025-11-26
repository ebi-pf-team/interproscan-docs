=========================
Trouble shooting and FAQs
=========================

How is InterProScan 6 different from InterProScan versions 4 and 5? How do I migrate?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

See the `Migrating to InterProScan 6 page <MigratingToI6.html>`__ for more information.

I keep running out of memory
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

See the `Improving Performance page <ImprovingPerformance.html>`__ for more information.

I get a "permission denied" error
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

``InterProScan`` 6 uses the `Nextflow <https://www.nextflow.io/>`_ workflow system for 
deployment.

On some systems, Nextflow requires root privileges to be able to create output directories and 
read and write its input/output files.

When Nextflow lacks the required permissions you may see an error similar to:

.. code-block:: bash

   ERROR ~ Error executing process > 'PARSE_SEQUENCE (1)'

   Caused by:
   Unable to create directory=.../InterProScan6/work/56/c74d6bbc6637c201a68afd84b75d27 -- check file system permissions

If you are running ``InterProScan`` 6 with Docker, you can provide root privileges via the 
``InterProScan`` ``nextflow.config`` file. Add the ``runOptions`` key to the ``docker`` settings, 
and assigning it the value ``--user root``:

.. code-block:: groovy

   docker {
      enabled = true
      mountFlags = 'Z'
      runOptions = '--user root'
   }

If you receive a "permission denied" error message that more closely resembles:

.. code-block:: bash

   Command error:
   docker: permission denied while trying to connect to the Docker daemon socket at unix:///var/run/docker.sock: Post "http://%2Fvar%2Frun%2Fdocker.sock/v1.24/containers/create?name=nxf-smAs6nFAn2qnYLr1PNVTxDV2": dial unix /var/run/docker.sock: connect: permission denied.
   See 'docker run --help'.

The issue is due to you local installation of Docker possessing insufficient privileges. Docker often 
requires root privileges in order to run a Nextflow pipeline.

Check that Docker and the associated ``InterProScan`` Docker image are configured correctly and have
all necessary privileges [`StackOverflow <https://stackoverflow.com/questions/48957195/how-to-fix-docker-got-permission-denied-issue>`_].

You can ensure the Docker socket has the necessary privileges by running the following command:

.. code-block:: bash

   sudo chmod 666 /var/run/docker.sock

InterProScan cannot access or fails to open files for writing
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

For example, you may receive an error resembling:

.. code-block:: bash

   Command error:

   Error: Failed to open output file hmmer_AntiFam.hmm.out for writing

This error typically arises due to ``InterProScan`` having insufficient privileges.

If you are using Docker to run ``InterProScan``, a potential fix is to provide root privileges to 
the Docker container that is deployed by Nextflow in order to run ``InterProScan`` (root privileges 
are often required), by editing the ``InterProScan`` ``nextflow.config`` file. Add 
the ``runOptions`` key to the ``docker`` settings, and assing it the value ``--user root``:

.. code-block:: groovy

   docker {
      enabled = true
      mountFlags = 'Z'
      runOptions = '--user root'
   }

Segmentation Fault
~~~~~~~~~~~~~~~~~~

If you a recieve an error such as the following:

.. code-block:: bash

   Command error:
   .command.sh: line 2:     7 Segmentation fault      (core dumped) /opt/hmmer3/bin/hmmsearch --cut_ga --cpu 1 -o 7.0._.antifam._.out AntiFam.hmm mini_test.1.fasta

This is generally due to HMMER being unable to find a necessary data file.
Make sure the data directory is correctly structured and populated and ``InterProScan`` is 
pointed to the correct data directory using the ``--data`` flag if not using the default data
directory location in the project dir.

Where can I find the XSD of the XML output?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The XML Schema Definition (XSD) is linked under the Extensible Markup
Language (XML) section of the
``InterProScan`` OutputFormats <OutputFormats.html>`__ page.

Can I use different binary versions than listed?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. ATTENTION::
   Swapping the binary versions is not recommended.

``InterProScan`` is designed to run using the same binaries that are employed by the supported 
versions of the member databases. This ensures that the
output results returned are as the member database intended. This is why,
for example, you will find multiple versions of HMMER bundled with ``InterProScan``, 
e.g. version 2 for the SMART and version 3 for Pfam analyses.

**Swapping the binary versions is not recommended.** ``InterProScan`` could fail, for example, 
if the input/output of the binary has changed and is no longer
recognised. Even if no errors are thrown, you would be running with an
unexpected binary and we cannot guarantee the results would match what
the analysis intended.

Which cluster does InterProScan support?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

``InterProScan`` version 6 employs the `Nextflow <https://www.nextflow.io/>`_ pipeline framework. 
Therefore, ``InterProScan`` theoretically provides support for 
GridEngine, SLURM, LSF, PBS, Moab and HTCondor batch schedulers and for Kubernetes, 
Amazon AWS, Google Cloud and Microsoft Azure platforms. However, 
SLURM is the only plaftorm we have tested here at the EBI, and provide an executor profile for.
You will need to create custom profiles for alternative systems.

Please 
see the `Profiles documentation <Profiles>`__ for more information.

Do I need to run InterProScan in cluster mode?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

To run ``InterProScan`` on a cluster you will need to specify the executor using the
``-profile`` option. For example, to run using SLURM and Singularity:

.. code-block:: bash

    nextflow run ebi-pf-team/interproscan6 \
        -profile singularity,slurm \
        --input <FASTA> \
        --datadir <DATADIR>

If the options in the built in profiles (stored in ``utilities/profiles``) are not
suitable for you system you will need to create and add your own profile. See the `Profiles documentation <Profiles>`__ for more information.

Is there a Galaxy wrapper for InterProScan?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

There is not one currently.

Documentation and contact details
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Galaxy Tool Shed link for ``InterProScan`` 6:
http://toolshed.g2.bx.psu.edu/view/bgruening/InterProScan6

Contact: Bjoern Gruening (bjoern.gruening@gmail.com)

Publication
^^^^^^^^^^^
Peter J.A. Cock, Björn A. Grüning, Konrad Paszkiewicz and Leighton
Pritchard (2013). Galaxy tools and workflows for sequence analysis with
applications in molecular plant pathology. PeerJ 1:e167
(http://dx.doi.org/10.7717/peerj.167)

I get Java errors on running InterProScan
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

If a simple test of ``InterProScan``  fails please check your installed
version of Java is suitable, see `installation
requirements <InstallationRequirements.html>`__ for more details. 
The latest version runs with Java 17 and later.

How to analyse a huge amount of protein sequences (>30000)?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Whereas ``InterProScan`` 5 could require the user to manually chunk their 
large input dataset, batching, unified parallelism, and scaling is automatically handled by 
``InterProScan`` version 6. To improve performance for very large datasets, it is recommended
to use a high-performance computing (HPC) environment and the ``bulk`` profile.

See the `Improving Performance page <ImprovingPerformance.html>`__ for more information.


Should I filter by e-value?
~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. ATTENTION::
   We do not recommend filtering the results by E-value.

The E-values are specific to each individual InterPro member database
and therefore, cannot be compared directly, and a single threshold cannot applied
to them all. This is because some member databases use the E-values for
post-processing (e.g. SMART, Panther), while others just output it as part of
their results and use other measures for filtering of results
(e.g. Pfam and the Hmmer GA cut-off). Therefore as far as ``InterProScan``
is concerned, if a match is in the output then it is a match!

Why do I see "Pre-calculated match lookup service failed - analysis proceeding to run locally"?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

This is a warning to say that the match lookup service you are trying to
use could not be used, therefore, ``InterProScan`` will calculate the results
locally on your system instead for all sequences in the input file. 
In this situation ``InterProScan`` will continue to run, however, this is 
likely to result in slower performance than normal.

This warning could occur because the lookup service your installation of
``InterProScan`` is configured to use is either:

* Not (or no longer) compatible with your version of ``InterProScan``.
* Is not accessible through your internet, proxy or firewall system configuration.
* Is temporarily down.

See `more information <PrecalculatedMatchLookup.rst>`_  about the lookup service to understand 
what is does and how to configure it.

What is the InterPro Match Lookup Service (MLS)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

``InterProScan`` is a computationally expensive program. InterPro integrates 
predictive information about proteins' function from
a number of partner resources, providing matches to more than 500 million protein
sequences, including all of the sequences in UniProtKB. We can take
advantage of this feature, and increase the speed of ``InterProScan``, by 
retrieving pre-calculated matches for sequences that are already found in InterPro. 

Specifically, when a sequence is submitted to ``InterProScan``, a 
MD5 checksum is calculated for the amino acid sequence
(or predicted ORF if a nucleic acid seqeuence is submitted).
``InterProScan`` then uses that checksum to check the 
:ref: `InterPro Match Lookup Service (MLS) <PrecalculatedMatchLookup.rst>`_ to see
whether it has already been encountered. If it has, the pre-calculated
results are returned to the user; if not, the ``InterProScan`` search
algorithms are run against the sequence.

By default, ``InterProScan`` has this option turned on. If you wish to turn
it off, you should add the ``--disable_precalc`` option at the command
line. By default an EBI-hosted instance of the
look-up service is used, alternatively you can download a
copy and running it locally. For more information, read the section on
`configuring the match lookup
service <#Configuring_the_Pre-calculated_Match_Lookup_Service>`_.

Why does InterProScan use hmmsearch not hmmscan?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Short answer: It is highly recommended that for the analysis of large datasets hmmsearch is used
instead of hmmscan, by the `HMMER developers <http://cryptogenomicon.org/hmmscan-vs-hmmsearch-speed-the-numerology.html>`_.

Explanation: For searches involving many sequences against many profile models, hmmsearch is 
vastly more efficient due to reduced input/data transfer requirements and better parallelization 
across compute cores. `TransDecoder <https://github.com/TransDecoder/TransDecoder/issues/94>`_ 
developers reported that hmmscan completed the search of 
~45,000 sequences against a HMM database in approximately 8 hours, while the same search was 
completed in less than an hour by hmmsearch.


.. NOTE::
   You can find the original blog post "hmmscan vs. hmmsearch speed: the numerology" 
   `here <http://cryptogenomicon.org/hmmscan-vs-hmmsearch-speed-the-numerology.html>`_.
