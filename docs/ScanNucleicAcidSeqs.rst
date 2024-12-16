How to Analyse Nucleic Sequences
================================

``InterProScan`` can take take advantage of Open Reading Frame (ORF) prediction 
in order to analyse nucleic acid sequences.

Specifically, ``InterProScan`` uses the ORF prediction tool 
``esl-translate`` from the `easel tool suite <https://github.com/EddyRivasLab/easel>`_ 
to generate predicted ORFs from an input nucleic acid FASTA file. These predicted ORFs are 
then used to generate hits. The predicted ORFs and their 
InterPro signature matches are associated with the respective input nucleic acid
sequence in the final output. 

The ``easel`` application itself and all its dependencies are integrated into ``InterProScan``.
Therefore, no additional configuration or installation is required.

How to scan nucleic acid sequences
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

To configure ``InterProScan`` to accept nucleic acid sequences as input, 
include the ``--nucleic`` flag in your ``InterProScan`` command:

.. code-block:: bash

    nextflow run ebi-pf-team/interproscan6 \
        -profile <slurm,lsf,local....docker,singularity,apptainer> \
        --input <path to input FASTA> \
        --datadir <interpro data dir> \
        --nucleic

.. ATTENTION::
    The input FASTA file must contain sequences of the same type, i.e. *only* protein sequences 
    or *only* nucleic sequences.

For example, to run ``InterProScan`` to analyse the nucleic acid sequences in the example input file 
``test_nt.fna``, with the match lookup service disabled and only running the analyses
``signalP``, ``Phobius`` (presuming these licensed tools are installed), CDD, SFLD, Panther and PFAM, 
using Docker locally, you could run the following command:

.. code-block:: bash

    nextflow run ebi-pf-team/interproscan6 \
        -profile docker,local \
        --input tests/data/test_nt.fna \
        --applications cdd,sfld,panther,pfam,signalp,phobius \
        --disablePrecalc \
        --nucleic

Improving performance when analysing nucleic acid sequences
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Multiple ORFs can be predicted from a single nucleic acid sequence. Therefore, analysing a 
FASTA file of nucleotide sequences often takes far longer than analysing a FASTA file 
that contains the same number of protein sequences.

To improve performance, you could employ a more stringent criteria by performing your own
ORF prediction analysis to reduce the number of ORFs to analyse.
