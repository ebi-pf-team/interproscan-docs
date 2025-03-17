Nucleic acid sequences scan
===========================

Translation into Open Reading Frames (ORFs)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

InterProScan 5 translates nucleotide sequences in six frames into individual ORFs using **esl-translate**
from the `Easel library <http://emboss.sourceforge.net/apps/cvs/emboss/apps/getorf.html>`__.
Easel is used by and distributed with the `HMMER software package <http://hmmer.org/>`__.
The **esl-translate** application itself and all of its dependencies are integrated
in InterProScan.

After translation, a parsing step will select the N longest ORFs and inputs
them for analysis. The number number of ORFs selected depends on an interproscan property with the default value of 8.
This means that analysing nucleotide sequences can take much longer than analysing protein sequences because 
each nucleotide sequence is translated into several protein sequences.


How can I scan nucleic acid sequences in InterProScan 5?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

To scan for nucleic acid sequences you must use the flag `-t n` or `--seqtype n` 
::
    -t,--seqtype <SEQUENCE-TYPE>   Optional, the type of the input sequences (dna/rna (n) or
                                   protein (p)).  The default sequence type is protein.

For example running:
::

    ./interproscan.sh -t n -i /path/to/nucleic_acid_sequences.fasta


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

Please note: non unique identifiers are automatically made unique by
adding '_sequential number' in the order of their appearance
(e.g. P11111 will be P11111_1 for the first protein sequence).


Improving performance
~~~~~~~~~~~~~~~~~~~~~

InterProScan does not select one best ORF from the input nucleotide sequence,
instead it takes the ORFs generated and select N longest ORFs and inputs
them for analysis. The number selected depends on the
binary.getorf.parser.filtersize property that has a default value of 8.
This means analysing nucleotide sequences can take much longer than analysing
protein sequences.

To improve InterProScan performance while running large nucleotide input
files (> 10,000 sequences) you can:

1. First translate your sequences externally and submit the protein sequences
   for interproscan analysis. This is the best approach.
   Besides esl-translate, other options may include
   `getorf <https://emboss.bioinformatics.nl/cgi-bin/emboss/help/getorf>`__
   or `transeq <https://emboss.bioinformatics.nl/cgi-bin/emboss/help/transeq>`__,
   both part of the `EMBOSS suite of bioinformatics tools <https://emboss.bioinformatics.nl/cgi-bin/emboss/>`__.

   If you use transeq then please use the -clean option to change STOP
   codon positions from \'\*\' to \'X\' because Interproscan does not accept
   sequences with the \'\*\' character.

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
