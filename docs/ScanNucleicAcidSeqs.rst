Nucleic acid sequences scan
===========================

The Open Reading Frame prediction tool
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

InterProScan 5 takes advantage of the Open Reading Frame (ORF)
prediction tool `Emboss
getorf <http://emboss.sourceforge.net/apps/cvs/emboss/apps/getorf.html>`__.
The getorf application itself and all of its dependencies are integrated
in InterProScan. You do not need to install the Emboss package on your
own, but you may use a local installation if you wish.

If you want to use a local installation you must edit the
interproscan.sh script. This script sets 2 environment variables for
Emboss getorf. Set these to the correct paths for your installation of
Emboss.

::

    # set environment variables for getorf
    export EMBOSS_ACDROOT=bin/nucleotide
    export EMBOSS_DATA=bin/nucleotide

In addition open and edit your properties file
(**interproscan.properties**), which you will find in your InterProScan
root directory. Search for the property '**binary.getorf.path**' and
change the path to your local **getorf** binary.

::

    binary.getorf.path=/path/to/bin/nucleotide/getorf

How can I scan nucleic acid sequences in InterProScan 5?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

::

    ./interproscan.sh -t n -i /path/to/nucleic_acid_sequences.fasta

or run the following commands:

::

    #translate the nucleic_acid_sequences
    ./bin/nucleotides/translate -i /path/to/nucleic_acid_sequences.fasta -o /path/to/output_orfs_sequences.fasta
    #if output_orfs_sequences.fasta has more than 32,000 sequences then chunk the file then send the chunks to InterProScan
    #run InterProScan on the translated output
    ./interproscan.sh -i /path/to/output_orfs_sequences.fasta

Which output formats are supported?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Supported output formats are GFF3 and XML, which allow you to trace back
from the match to the position inside your nucleic acid sequence. Please not
that the TSV format is not available for nucleic acid sequence analysis.

Redundant sequences and identifiers in your FASTA file
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

InterProScan 5 is able to handle FASTA file entries with the same
sequence, but different identifiers. For instance you have the following
2 sequences in your input file:

::

    >sequence_1
    ABC
    >sequence_2
    ABC

InterProScan 5 will condense these into a single sequence with two
identifier cross-references in the XML output file:

::

    <nucleotide-sequence>
            <sequence md5="e9b174d63adc63bab79c90fdbc8d1670">ABC</sequence>
            <xref id="sequence_1"/>
            <xref id="sequence_2"/>
            <orf strand="SENSE" start="1" end="3">
    ...

and in the GFF3 output:

::

    ##sequence-region sequence_1|sequence_2 1 3
    sequence_1|sequence_2   provided_by_user    nucleic_acid    1   3
    ...

Entries with the same identifier and the same sequence will be merged
into one.

Please note: non unique identifiers are not supported. InterProScan 5
will exit (with exit code 0) and will print out a list of all non unique
identifiers.

Improving performance
~~~~~~~~~~~~~~~~~~~~~

InterProScan does not select one best ORF from the getorf output,
instead it takes the ORFs generated and select N longest ORFs and inputs
them for analysis. The number selected depends on the
binary.getorf.parser.filtersize property mentioned below. The default is 8. This
means analysing nucleotide sequences can take much longer than analysing
protein sequences.

To improve InterProScan performance while running large nucleotide input
files (> 10,000 sequences) you can:

1. First use an external program to translate your input. This is the
   best approach. There are various options, one of which is
   emboss-transeq
   (http://emboss.open-bio.org/rel/rel6/apps/transeq.html) from emboss.
   If you use transeq then please use the -clean option to change STOP
   codon positions from '*' to 'X' because Interproscan does not accept
   sequences with the '*' character.

and/or...

2. Chunk the input and then send the chunks to InterProScan. For tips on
   configuring the general InterProScan CPU usage see also `improving
   performance <ImprovingPerformance.html>`__.

Selecting the ORFs to analyse
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

For improved performance, Interproscan will select the longest 8 ORFs
predicted for each nucleic acid sequence. This can be changed using the
new "binary.getorf.parser.filtersize" setting in the
interproscan.properties file

::

    binary.getorf.parser.filtersize=8
