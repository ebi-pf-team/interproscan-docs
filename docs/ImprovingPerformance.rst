Improving performance
=====================

If InterProScan is taking a long time to run, or you just want to improve on the
run time you are getting, then consider some of the following:

Review your CPU (and memory) command options
------------------------------------------
By default InterProScan uses 8 cpu cores on your machine. Most of the times this
configuration is sufficient. However, if you have more cores available
and you have more memory to support more threads, then you can change the number  of
cpu cores used by adding the option below to the InterProScan command line, where N
is the desired number of cores
::
    -cpu N

You have to remember, the more cores you specify, the more memory InterProScan
will require to run successfully.  Here are some observed numbers that may act
as a guide, but you may have to experiment for your own data. The input sequences
were taken from `UniProt <https://www.uniprot.org>`__

.. list-table:: Run time statistics for selected input
   :widths: 15 22 23 20 20
   :header-rows: 1

   * - -cpu
     - max memory used (GB)
     - input sequence count
     - input sequence size (MB)
     - run time
   * -   16
     -    10
     -  16,000
     -    6
     -   4 hrs
   * -   16
     -    14
     - 160, 000
     -   56
     -   12hrs


Consider chunking large input files
-----------------------------------

If your FASTA input files contains a large number of sequences
say over 160, 0000 protein sequences, then you
may consider splitting your input into smaller chunks (depends on
resources, but batches of 80,000 protein sequences is a suggested
starting point). You can then submit the smaller input files to
InterProScan and process the results afterwards.

For DNA/RNA sequences a much smaller number is suggested (e.g. 12,000
sequences). However for improved performance you could translate these
using an external tool and then submit the necessary protein sequences
instead, see `running nucleic acid sequences <ScanNucleicAcidSeqs.html>`__
for more information.

Review your command line input options
--------------------------------------

Do you need all the output InterProScan supplies by default? See `How to
run InterProScan <HowToRun.html>`__ for more details, for example you may
consider options such as:

* Which result data are you interested in, do you require all applications (see `-appl option <HowToRun.html#appl-applications-application-name-optional>`__ )?
* Do you require the residue level annotation? If not, this calculation can be disabled with the -dra option.
* Make use of the default lookup service, or your own local lookup service to avoid the need for calculating known results again (on by default, `read more <LocalLookupService.html#what-is-the-interproscan-5-lookup-service>`__).



How to configure CPU usage? - Example cases
-------------------------------------------

Case 1: Running InterProScan in STANDALONE mode (default)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

You want to analysis sequences on a single machine using a certain
amount of available cores.

Let's say your super machine has 40 cores available and you want to use
all or most of the cores. How would you do that?

In your InterProScan properties file edit the following properties to:

::

    #Number of embedded workers at start time
    number.of.embedded.workers=1
    #Maximum number of embedded workers
    maxnumber.of.embedded.workers=35

The **number of embedded workers** is how many you will initially start
with, while the **maxnumber** is the highest number of workers that it
will run at a time. Each embedded worker will run on one core and could
start a binary, which will use a maximum of 1 core(see above e.g. HMMER3
binary). In addition the master process will run on one core. So the
best way to find out how to set the maxnumber of embedded workers is to
subtract 1 core for the master process and 1 core for each embedded
worker from the total number of cores available on your machine.

+-----------------------------+------------------+
| **Threads**                 | **Cores used**   |
+=============================+==================+
| master process              | 1                |
+-----------------------------+------------------+
| 35 embedded worker (35x1)   | 35               |
+-----------------------------+------------------+
| **total # of cores**        | **36**           |
+-----------------------------+------------------+

So if there are 40 cores on your machine then you should set this number
to 35. You could also change the number of CPUs used by the binaries,
which will have an impact on how many embedded workers you could maximum
have.

For example, if you set the cpu usage for all binaries to 4, then you
would be able to set the maximum number of embedded workers 9 (1 master
core + (9 \* 4) embedded worker cores = 36 cores total). This would
allow less binary jobs to run simultaneously, but each would run faster
than if they were using 1 cpu each.

Case 2: Running InterProScan in CLUSTER mode
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

You want to analysis sequences on a cluster/farm and you would like to
set the number of reserved cores for each node.

See `Running InterProScan in CLUSTER mode <ClusterMode.html>`__

Configure to analyse fewer ORFs (applies to nucleic acid sequences only)
------------------------------------------------------------------------

For nucleic acid sequences, consider `reducing the number of ORFs to
analyse <ScanNucleicAcidSeqs.html#selecting-the-orfs-to-analyse>`__.
