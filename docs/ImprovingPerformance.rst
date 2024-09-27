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
