Improving performance
=====================

If InterProScan is taking a long time to run, or you just want to improve on the
run time you are getting, then consider some of the following:

Review your CPU and memory command options
------------------------------------------

By default ``InterProScan`` uses 1 cpu core per job, and 2 cpu for PRINTS. Additionally, 
most jobs are assigned 6GB, except SignalP and PRINTS which are assigned 16 and 32GB, respectively. 

These values are defined in the executor profiles (located in ``utilities/profiles``): local and slurm.

You could try assigning more memory to the slower running processes. 

Consider chunking large input files
-----------------------------------

If your FASTA input files contains a large number of sequences say over 160,0000 protein sequences,
then you may consider splitting your input into smaller chunks (thus depends on resources, but batches of
100,000 protein sequences is a suggested starting point). You can then submit the smaller input files to
``InterProScan``` and process the results afterwards.

For DNA/RNA sequences a much smaller number is suggested (e.g. 12,000 sequences).
However for improved performance you could translate these using an external tool
and then submit the necessary protein sequences instead, see running nucleic acid sequences for more information.

Increase the queue size
-----------------------

When running ``InterProScan`` on the cluster the Nextflow parameter ``QueueSize`` tests the total number 
of parallel jobs than ``InterProScan`` can handle. Increasing this value can allow ``InterProScan`` 
to run more jobs in parallel, thus reducing the total run time. 

The ``QueueSize`` is set in each of the executor profiles in ``utilities/profiles/``.

Reassess the batch size
-----------------------

We selected a batch size we found to be optimal when running an analysis will all member (licensed and 
built-in). However, the total runtime may be reduced by using an alternative batch size when 
running analyses with a subset of databases.

The batch size is defined in ``InterProScan`` ``nextflow.config`` file (found within the root 
of the project directory).

Review your command line input options
--------------------------------------

See `How to
run InterProScan <HowToRun.html>`__ for more details, for example you may
consider options such as:

**Which result data are you interested in, do you require all applications?**  Use the 
``--application`` flag to define a set of member databases/applications to use in the analysis.

**Make use of the default lookup service, or your own local lookup service to avoid the need 
for calculating known results again**. Do not include the ``--disable_precalc`` flag in your 
``InterProScan`` command.

Configure to analyse fewer ORFs (applies to nucleic acid sequences only)
------------------------------------------------------------------------

For nucleic acid sequences, consider `reducing the number of ORFs to
analyse <ScanNucleicAcidSeqs.html#configuring-the-orf-prediction>`__.

Update to Java 19 or 21
-----------------------

`Virtual threads <https://www.infoq.com/articles/java-virtual-threads/>`__ were introduced in Java 19 and have been shown
to potentially reduce the memory requirements of a Nextflow pipeline (as discussed in this
`Seqera blog <https://seqera.io/blog/optimizing-nextflow-for-hpc-and-cloud-at-scale/>`__).

From Nextflow +23.05.0, you can enable virtual threads by using Java 19 or later and setting the ``NXF_ENABLE_VIRTUAL_THREADS``
environment variable to true. For Nextflow version +23.10.0, when using Java 21, virtual threads are enabled by default.
