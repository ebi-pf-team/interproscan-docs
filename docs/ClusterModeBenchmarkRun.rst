Cluster mode benchmark run
==========================

We have ran InterProScan 5 (I5) in CLUSTER mode against a complete
Escherichia coli proteome to give you some benchmark figures in terms of
analysis runtime.

This documentation could be seen as a reference point for runtime, but
also on how to set up I5 appropriate for speed improvement.

Benchmark run setup
-------------------

Which version of InterProScan 5 (I5) was used for this run?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

`5.7-48 <Interproscan5_7_48_ReleaseNotes>`__

How was the set of input sequences assembled for this run?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

For this run we decide to run I5 against the complete proteome of
Escherichia coli (Taxon 83333). We've downloaded the proteome from the
`Reference proteomes <http://www.ebi.ac.uk/reference_proteomes>`__
website (RELEASE 2014\_04).

RELEASE 2014\_04 is based on UniProt Release 2014\_04, Ensembl release
75 and Ensembl Genome release 21. The E.coli proteome for this release
contains 4303 protein sequences. You can download the sequence file
`here <https://drive.google.com/file/d/0B46zw6xrjt1VY2Z5ZDZfNHRBQU0/view?usp=sharing>`__.

Which I5 command was used for this run?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

We switched off the pre-calculated match lookup service and turned on
the CLUSTER mode.

::

    ./interproscan.sh
    -i 83333.fasta
    -dp
    -f tsv,html
    --goterms
    -mode cluster
    -clusterrunid benchmark-5.7-48.0

How does the interproscan.properties file look like?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The default settings are very conservative. To speed up the CLUSTER mode
we've added or changed the values of the following 6 attributes within
the DEFAULT interproscan.properties file:

::

    grid.throttle=false
    master.steps.to.consumer.ratio=1
    steps.to.consumer.ratio=1
    max.tier.depth=2
    thinmaster.number.of.embedded.workers=5
    thinmaster.maxnumber.of.embedded.workers=5

The full setting file can be found
`here <LsfClusterModeSetupForBenchmark>`__.

On which cluster/farm did we run I5?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

We ran I5 on our internal LSF cluster. As of 1st of May 2014, there are
approximatley 680 nodes comprising 16,000 hyper-threaded CPU cores.

Benchmark run outcome
---------------------

\| \| **Run 1** \| **Run 2** \| **Run 3** \|
\|:\|:----------\|:----------\|:----------\| \|Wall clock time\|1h 37min
\| N/A \| N/A \| \|Max number of workers\| 45 \| N/A \| N/A \|
