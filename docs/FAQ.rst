FAQ
===

What should I do if one of the binaries included with InterProScan 5 doesn't work on my system?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Please see the section `Compiling binaries <CompilingBinaries>`__ for
instructions on how to compile the various binaries on your own system.

Where can I find the XSD of the XML output?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The XML Schema Definition (XSD) is linked under the Extensible Markup
Language (XML) section of the
`InterProScan5OutputFormats <InterProScan5OutputFormats>`__ page.

How is InterProScan 5 different from InterProScan 4? How do I migrate?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

InterProScan 5 differs from `InterProScan
v4.x <ftp://ftp.ebi.ac.uk/pub/software/unix/iprscan/index.html>`__ in
the following ways:

-  New analysis type: `Phobius <http://phobius.sbc.su.se/>`__ for
   transmembrane and signal peptide prediction
-  New feature: ability to map InterPro results back to the original
   nucleotide sequences that were submitted
-  New feature: option to look up biological pathways that the protein
   is potentially involved in
-  New output formats: "IMPACT" XML format and GFF3.0
-  Improved graphical (HTML and SVG) representations of the protein
   matches

InterProScan 4.8 is no longer supported or updated. For more details on
how to migrate to InterProScan 5 see the `migration
page <MigratingInterProScanVersions>`__.

Which cluster does InterProScan 5 support?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

In theory InterProScan 5 is written flexible enough to run on any
cluster platform and not only on LSF and SGE. But LSF and SGE are the
only platforms we can test here at the EBI. We had feedback from users
who run it successfully on a PBS cluster. For further info on how to
configure your cluster version please follow the
`documentation <InterProScan5ClusterMode>`__.

Can I use different binary versions than listed?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

InterProScan 5 is designed to run with the same binaries used by the
supported member database analysis versions. This ensures that the
output results returned are as the member database intended. This is why
for example you will find multiple versions of HMMER (e.g. for the SMART
and Pfam analyses) bundled with InterProScan and referenced in the
interproscan.properties configuration file.

Swapping the binary versions is not recommended. InterProScan could fail
(e.g. if the input/output of the binary has changed and is no longer
recognised). Even if no errors are thrown, you would be running with an
unexpected binary and we cannot guarantee the results would match what
the analysis intended.

If you are having problems running the provided versions of certain
binaries on your system, please follow these
`instructions <CompilingBinaries>`__.

Is there Galaxy has a wrapper for InterProScan 5?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Do you want to add InterProScan 5 to your
`Galaxy <http://galaxyproject.org/>`__ analysis pipeline? You can find
the wrapper for InterProScan 5 on
`GitHub <https://github.com/peterjc/bgruening_galaxytools/tree/master/iprscan5>`__.

When using InterProScan 5 with Galaxy, the cluster integration is done
via Galaxy, which means you cannot use InterProScan 5's in-built CLUSTER
mode.

Documentation and contact details
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Galaxy Tool Shed link for InterProScan 5:
http://toolshed.g2.bx.psu.edu/view/bgruening/interproscan5

Contact: Bjoern Gruening (bjoern.gruening@gmail.com)

Publication
^^^^^^^^^^^

Peter J.A. Cock, Björn A. Grüning, Konrad Paszkiewicz and Leighton
Pritchard (2013). Galaxy tools and workflows for sequence analysis with
applications in molecular plant pathology. PeerJ 1:e167
(http://dx.doi.org/10.7717/peerj.167)

I get Java errors on running InterProScan 5
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

If a simple test of InterProScan 5 fails please check your installed
version of Java is suitable, see `installation
requirements <InstallationRequirements>`__ for more details.

How to analyse a huge amount of protein sequences (>30000)?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The following guidance I would say is good practice, when you use
InterProScan 5 to annotate large sequence sets. To give you an example
about InterProScan's run time, inhouse we are able to annotate a
complete Escherichia coli proteome (~3.000 protein sequences) on our
farm (standalone mode) within ~2hours. Other sequence sets of 16,000
protein sequences have taken ~6 hours on an a machine with 8 cores and
8GB RAM.

If you want to annotate huge ammounts of protein sequences we would
strongly recommend to chunk your input sequences into chunks for lets
say 30,000 sequences. If you are analysing nucleic acid sequences, the
chunk size should be even smaller. And then you would run individual
InterProScan jobs for each chunk file. That way you make sure you get
intermediate results and if lets say your InterProScan program crashes
on half way you do not lose everything.

Should I filter by e-value?
~~~~~~~~~~~~~~~~~~~~~~~~~~~

The e-values are specific to each individual InterPro member database
and therefore cannot be compared directly, or a single threshold applied
to them all. This is because some member databases use the e-values for
post-processing (e.g. SMART, Panther), others just output it as part of
their results but actually use other measures for filtering of results
(e.g. Pfam and the Hmmer GA cut-off). Therefore as far as InterProScan
is concerned, if a match is in the output then it is a match!

Why do I see "Pre-calculated match lookup service failed - analysis proceeding to run locally"?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

This is a warning to say that the match lookup service you are trying to
use could not be used, therefore InterProScan will calculate the results
locally on your system instead. In this situation InterProScan will
continue run, however this is likely to result in slower performance
than normal.

This warning could occur because the lookup service your installation of
InterProScan is configured to use is either: \* Not (or no longer)
compatible with your version of InterProScan. \* Is not accessible
through your internet, proxy or firewall system configuration. \* Is
temporarily down.

See `more
information <LocalLookupService#what-is-the-interproscan-5-lookup-service>`__
about the lookup service to understand what is does and how to configure
it.
