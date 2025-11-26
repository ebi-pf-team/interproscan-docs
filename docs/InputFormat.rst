=============
Input formats
=============

``InterProScan`` only accepts a single FASTA file as its input, although this input 
FASTA file can contain multiple sequences. For example:

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

The following characters are not allowed in sumitted sequences: 

* ``-``
* ``.`` - Note this gap character is allowed in nucleic acid sequences
* ``_``

In addition, Phobius does not allow the following characters in sequences:

* ``*``
* ``o``
* ``x``
* ``u``
* ``z``
* ``j``

If any of these characters are found in the input sequences, ``InterProScan`` will raise
an error and terminate.
