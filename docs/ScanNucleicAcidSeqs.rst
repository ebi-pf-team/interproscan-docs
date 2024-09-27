How to Analyse Nucleic Sequences
================================

``InterProScan`` can take take advantage of Open Reading Frame (ORF) prediction 
in order to analyse nucleic acid sequences.

Specifically, ``InterProScan`` uses the ORF prediction tool 
``esl-translate`` from the `easel tool suite <https://github.com/EddyRivasLab/easel>`_ 
to generate predicted ORFs from an input nucleic acid FASTA file. These predicted ORFs are 
then used to generate hits. The predicted ORFs and their 
InterPro signature matches are then associated with the respective input nucleic acid 
sequence in the final output. 

The ``easel`` application itself and all its dependencies are integrated into ``InterProScan`` via 
the ``InterProScan`` Docker image. Therefore, no additional configuration or installation is
required.

How to scan nucleic acid sequences
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

To configure ``InterProScan`` to accept nucleic acid sequences as input, 
include the ``--nucleic`` flag in your ``InterProScan`` command:

.. code-block:: bash

    nextflow run ebi-pf-team/interproscan6 \
        --input <path to input FASTA> \
        --nucleic \
        -profile <profiles>

.. ATTENTION::
    The input FASTA file must contain sequences of the same type, i.e. *only* protein sequences 
    or *only* nucleic sequences.

For example, to run ``InterProScan`` to analyse the nucleic acid sequences in the example input file 
``test_nt_seqs.fasta``, with the match lookup service disabled and only running the analyses 
``signalP``, ``Phobius`` (presuming these licensed tools are installed), CDD, SFLD, Panther and PFAM, 
using the Docker ``interproscan6`` image locally, you could run the following command:

.. code-block:: bash

    nextflow run ebi-pf-team/interproscan6 \
        --input utilities/test_files/test_nt_seqs.fasta
        --applications cdd,sfld,panther,pfam,signalp,phobius \
        --disable_precalc \
        --nucleic \
        -profile docker,local

Configuring the ORF prediction
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

You can configure the prediction of the ORFs by ``esl-translate`` by updating the 
relevant ``translate`` parameters in the ``InterProScan`` ``nextflow.config`` file:

.. code-block:: groovy

    translate {
        strand = 'both'
        methionine = false
        min_len = 20
        genetic_code = 1
    }

**Strand:** The DNA strand(s) to be translated. It can be set to:

* ``'both'``
* ``'plus'``
* ``'minus'``

**Methionine:** Whether the predicted ORFs start with a methionine (M):

* ``false`` - use the initation codon
* ``true`` - all ORFs start with 'M'

**Min_len (minimum length):** The minimum length a predicted ORF can be. (Any integer)

**Genetic code:** The number of the genetic code to use.

Choosing the genetic code
~~~~~~~~~~~~~~~~~~~~~~~~~

``esl-translate`` supports several genetic codes. The genetic code of interest 
can be specified by modifying the value of the ``genetic_code`` key under the ``translate`` 
parameter in the ``InterProScan`` ``nextflow.config`` file.

The supported genetic codes are:

.. raw html

    <table>
    <thead>
        <tr>
        <th>ID</th>
        <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr>
        <td>1</td>
        <td>Standard</td>
        </tr>
        <tr>
        <td>2</td>
        <td>Vertebrate mitochondrial</td>
        </tr>
        <tr>
        <td>3</td>
        <td>Yeast mitochondrial</td>
        </tr>
        <tr>
        <td>4</td>
        <td>Mold, protozoan, coelenterate mitochondrial; Mycoplasma/Spiroplasma</td>
        </tr>
        <tr>
        <td>5</td>
        <td>Invertebrate mitochondrial</td>
        </tr>
        <tr>
        <td>6</td>
        <td>Ciliate, dasycladacean, Hexamita nuclear</td>
        </tr>
        <tr>
        <td>9</td>
        <td>Echinoderm and flatworm mitochondrial</td>
        </tr>
        <tr>
        <td>10</td>
        <td>Euplotid nuclear</td>
        </tr>
        <tr>
        <td>11</td>
        <td>Bacterial, archaeal; and plant plastid</td>
        </tr>
        <tr>
        <td>12</td>
        <td>Alternative yeast</td>
        </tr>
        <tr>
        <td>13</td>
        <td>Ascidian mitochondrial</td>
        </tr>
        <tr>
        <td>14</td>
        <td>Alternative flatworm mitochondrial</td>
        </tr>
        <tr>
        <td>16</td>
        <td>Chlorophycean mitochondrial</td>
        </tr>
        <tr>
        <td>21</td>
        <td>Trematode mitochondrial</td>
        </tr>
        <tr>
        <td>22</td>
        <td>Scenedesmus obliquus mitochondrial</td>
        </tr>
        <tr>
        <td>23</td>
        <td>Thraustochytrium mitochondrial</td>
        </tr>
        <tr>
        <td>24</td>
        <td>Pterobranchia mitochondrial</td>
        </tr>
        <tr>
        <td>25</td>
        <td>Candidate Division SR1 and Gracilibacteria</td>
        </tr>
    </tbody>
    </table>

Improving performance when analysing nucleic acid sequences
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Multiple ORFs can be predicted from a single nucleic acid sequence. Therefore, analysing a 
FASTA file of nucleotide sequences often takes far longer than analysing a FASTA file 
that contains the same number of protein sequences.

To improve performance, you could employ a more stringent criteria for identifying ORFs, including
altering the minimum length and whether all ORFs must start with methionine.
By default, ``esl-translate`` is configured to identify ORFs with a minimum length of 20 residues. 
