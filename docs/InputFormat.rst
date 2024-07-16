Input formats
=============

Supported input file format
---------------------------

InterProScan 5 supports the FASTA file format.

An example of a simple FASTA format file containing unaligned sequences:

::

    > seq1 Description of seq1.
    AGTACGTAGTAGCTGCTGCTACGTGCGCTAGCTAGTACGTCA
    TAGTA
    > seq2 Description of seq2.
    CGATCGATCGTACGTCGACTGATCGTAGCTACGTCGTACGTAG
    CATCGTCAGTTACTGC

Supported sequence format
-------------------------

InterProScan 5 supports **unaligned sequences** only. Sequences should
contain only valid IUPAC amino acid or nucleic acid characters. In
addition gap ('-'), period ('.'), asterisk or underscore symbols are not
allowed and should produce warnings and InterProScan will exit
immediately.

Example for supported protein sequence:

::

    MPIGSKERPTFFEIFKTRCNKADLGPISLNWFEELSSEAPPYNSEPAEESEHKNNNYEPN

Example for supported nucleic acid sequence:

::

    atgaaatataaacgcattgtgtttaaagtgggcaccagcagcctgaccaacg

Unsupported sequences:

::

    -RFLLLSLARFSNNRFGVQLLQIANVNLKVRRYG (illegal gap character at the start)

    RFLLLSL--ARFSNNRFGVQLLQIANVNLKVRRYG (illegal gap character in the middle)

    RFLLLSLARFSNNRFGVQLLQIANVNLKVRRYG* (illegal asterix character at the end)

    RFLLLSL_ARFSNNRFGVQLLQIANVNLKVRRYG (illegal underscore character)

    RFLLLSL.ARFSNNRFGVQLLQIANVNLKVRRYG (illegal period character)
