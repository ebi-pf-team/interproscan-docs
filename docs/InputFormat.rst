=============
Input formats
=============

``InterProScan`` only accepts a single FASTA file as its input. The input 
FASTA file can contain multiple sequences.

For example, here is an extract of a simple FASTA format file containing unaligned sequences:

::

    > seq1 Description of seq1.
    AGTACGTAGTAGCTGCTGCTACGTGCGCTAGCTAGTACGTCA
    TAGTA
    > seq2
    CGATCGATCGTACGTCGACTGATCGTAGCTACGTCGTACGTAG
    CATCGTCAGTTACTGC
    > sp|seq3 Description of seq3
    CGATCGATCGTACGTCGACTGATCGTAGCTACGTCGTACGTAG
    CATCGTCAGTTACTGCATGGTT

.. ATTENTION::
    The input FASTA file must contain sequences of the same type, i.e. *all* protein sequences 
    or *all* nucleic sequences.

Illegal characters
------------------

Some analysis methods do not support specific characters within the input sequences. The table
below lists all the illegal characters for each of the member databases whom (as far as we 
are aware) haven non-tolerated characters. If ``InterProScan`` detects any of these characters 
in the input to the respective member database it should produce warnings and exit immediately.

.. WARNING::
    We cannot guarantee that this is an exhaustive list.

.. list-table::
   :widths: 30 70
   :header-rows: 1

   * - Member DB
     - Illegal characters
   * - AntiFam
     - \-
   * - FunFam
     - \- _ .
   * - Gene3D
     - \- _ .
   * - HAMAP
     - \- _ .
   * - NCBIFAM
     - \-
   * - Panther
     - \-
   * - Pfam
     - \-
   * - Phobius
     - \- _ . * o x u z j
   * - PIRSR
     - \-
   * - PIRSF
     - \-
   * - PROSITE Profiles
     - \- _ .
   * - SFLD
     - \- _ .
   * - SUPERFAMILY
     - \-

Here is an example of a supported protein sequence...

::

    MPIGSKERPTFFEIFKTRCNKADLGPISLNWFEELSSEAPPYNSEPAEESEHKNNNYEPN

and a supported nucleic acid sequence...

::

    atgaaatataaacgcattgtgtttaaagtgggcaccagcagcctgaccaacg

and examples of unsupported sequences:

::

    -RFLLLSLARFSNNRFGVQLLQIANVNLKVRRYG (illegal gap character at the start)

    RFLLLSL--ARFSNNRFGVQLLQIANVNLKVRRYG (illegal gap character in the middle)

    RFLLLSLARFSNNRFGVQLLQIANVNLKVRRYG* (illegal asterix character at the end)

    RFLLLSL_ARFSNNRFGVQLLQIANVNLKVRRYG (illegal underscore character)

    RFLLLSL.ARFSNNRFGVQLLQIANVNLKVRRYG (illegal period character)
