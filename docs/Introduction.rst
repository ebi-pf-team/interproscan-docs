Introduction
============

What is InterProScan?
~~~~~~~~~~~~~~~~~~~~~

`InterPro <http://www.ebi.ac.uk/interpro/>`__ is a database which
integrates together predictive information about proteins' function from
a number of partner resources, giving an overview of the families that a
protein belongs to and the domains and sites it contains.

Users who have novel nucleotide or protein sequences that they wish to
functionally characterise can use the software package InterProScan to
run the scanning algorithms from the InterPro database in an integrated
way. Sequences are submitted in FASTA format. Matches are then
calculated against all of the required member database's signatures and
the results are then output in a variety of formats.

Supported platforms
~~~~~~~~~~~~~~~~~~~

-  64-bit Linux that meets the :ref:`Installation requirements`

*There are no versions planned for* **Windows** or **Apple (MAC OS X)**
*operating systems.*

This is due to constraints in the various third-party binaries that
InterProScan runs.

To install and run InterProScan
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

For more information about using InterProScan please see the page links
on the right, for example `how to download a copy <HowToDownload.html>`__ and
`how to run InterProScan <HowToRun.html>`__.

LSF cluster users
^^^^^^^^^^^^^^^^^

As an alternative to the default "standalone" mode, Interproscan 5
allows components of the analysis to be farmed out on an LSF or SGE
cluster. Full details of this can be found in
:ref:`Running InterProScan 5 in Cluster Mode`.
