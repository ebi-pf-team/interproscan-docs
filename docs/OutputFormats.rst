==============
Output formats
==============

``InterProScan`` version 6 you can the retrieve output in the following formats:

-  `TSV <OutputFormats.html#tab-separated-values-format-tsv>`__: A simple tab-delimited file format
-  `JSON <OutputFormats.html#javascript-object-notation-json>`__: Full output of results in JSON format
-  `XML <OutputFormats.html#extensible-markup-language-xml>`__: The ``InterProScan`` XML format

This page provides a summary of the data structure of each output file.

General notes
-------------

Continuous and discontinuous (dc-)status
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The ``dc-status`` refers to continuous nature of a domain 
hit in some member databases. At the present only Gene3D and FunFam are able to detect discontinious domains.

If a domain is not  continuous, i.e. is broken up into fragments, each 
of the fragments are represented under the ``location-fragments`` key and are labelled as 
"C_TERMINAL_DISC" (for the most c-terminal fragment), "N_TERMINAL_DISC" (for the most n-terminal fragment), 
and "NC_TERMINAL_DISC" (for all other fragments) - where "DISC" is short for "discontinuous". For example:

.. code-block:: JSON

    {
        "location-fragments" : [ {
            "start" : 84,
            "end" : 134,
            "dc-status" : "NC_TERMINAL_DISC"
            }, {
            "start" : 7,
            "end" : 43,
            "dc-status" : "C_TERMINAL_DISC"
            }, {
            "start" : 477,
            "end" : 529,
            "dc-status" : "N_TERMINAL_DISC"
            }, {
            "start" : 203,
            "end" : 297,
            "dc-status" : "NC_TERMINAL_DISC"
        } ]
    }

One-hit limit for Panther
~~~~~~~~~~~~~~~~~~~~~~~~~

The output from HMMER3 against the HMM models of Panther is post-processed to 
select only the best homologous family. Therefore, there is a maximum of one domain hit 
for each Panther signature in a protein sequence. Owing to this the E-value and Score and listed 
under the ``signature`` key, not the ``locations`` key.

.. TIP::
    To understand the meaning of the values in the ``InterProScan`` output for member databases 
    that implement ``hmmsearch`` from HMMER3, we recommend referring to the official 
    `HMMER3 documentation <http://eddylab.org/software/hmmer/Userguide.pdf>`_, specifically 
    under the "Step 2: search the sequence database with hmmsearch" header.

Tab-separated values format (TSV)
---------------------------------

The ``TSV`` file presents the data in a simple tab delimited format, that only includes sequences 
with domain matches.

Data in the TSV file
~~~~~~~~~~~~~~~~~~~~

The TSV format presents the match data in columns as follows:

1.  Protein accession (e.g. P51587)
2.  Sequence MD5 digest (e.g. 14086411a2cdf1c4cba63020e1622579)
3.  Sequence length (e.g. 3418)
4.  Member database (e.g. Pfam / PRINTS / Gene3D)
5.  Signature accession (e.g. PF09103 / G3DSA:2.40.50.140)
6.  Signature description (e.g. BRCA2 repeat profile)
7.  Start location (match location start in the query sequence)
8.  Stop location (match location end in the query sequence)
9.  Score - is the E-value (or score) of the match reported by the member
    database (e.g. 3.1E-52)
    * E-value for AntiFam, Cath-Gene3D, FunFam, NCBIFam, PANTHER, Pfam, PIRSF, PRINTS, SFLD, SMART, SUPERFAMILY, CDD
    * Score for HAMAP, PROSITE ProSiteProfiles
    * '-' for Coils, MobiDB-lite, Phobius, PROSITE Patterns, SignalP, TMHMM
10. Status - is the status of the match (T: true)
11. Date - is the date of the run (format ``DD-MM-YYYY``)
12. Associataed InterPro entry accession (e.g. IPR002093)
13. Associated InterPro entry description (e.g. BRCA2 repeat)
14. Pipe-separated list of GO annotations with their source(s), e.g. GO:0005515(InterPro)|GO:0006302(PANTHER)|GO:0007195(InterPro,PANTHER). This is an optional column; only displayed if the ``--goterms`` option is switched on
15. Pipe-separated list of pathways annotations, e.g. REACT\_71. This is an optional column; only displayed if the ``--pathways`` option is switched on

An example TSV file
~~~~~~~~~~~~~~~~~~~

Below is an example of a TSV file that includes the optional GO terms and pathways columns:

::

    P51587  14086411a2cdf1c4cba63020e1622579    3418    Pfam    PF09103 BRCA2, oligonucleotide/oligosaccharide-binding, domain 1    2670    2799    7.9E-43 T   15-03-2013    -    -
    P51587  14086411a2cdf1c4cba63020e1622579    3418    ProSiteProfiles PS50138 BRCA2 repeat profile.   1002    1036    0.0 T   18-03-2013  IPR002093   BRCA2 repeat    GO:0005515|GO:0006302    -
    P51587  14086411a2cdf1c4cba63020e1622579    3418    Gene3D  G3DSA:2.40.50.140       2966    3051    3.1E-52 T   15-03-2013    -    -
    UPI0004FABBC5	92e4b89dd86f8ab828f57121f6d7d460	257	PRINTS	PR01914 Neurotrophin-3 signature	81	95	2.0E-26	T	28-03-2024	IPR015578	Neurotrophin-3	GO:0005165(InterPro)	Reactome:R-BTA-9034013|Reactome:R-BTA-9034793|Reactome:R-BTA-9603381|Reactome:R-HSA-9025046|Reactome:R-HSA-9034013|Reactome:R-HSA-9034015|Reactome:R-HSA-9034793|Reactome:R-HSA-9034864|Reactome:R-HSA-9603381|Reactome:R-MMU-9034013|Reactome:R-MMU-9034793|Reactome:R-MMU-9603381|Reactome:R-RNO-9034013|Reactome:R-RNO-9034793|Reactome:R-RNO-9603381

.. NOTE::
    If a value is missing in a column, for example, the match has no InterPro annotation, a '-' is displayed.

An example TSV file (Nucleic)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Below is an example of a TSV file that was generated using nucleic acid sequences as input:

::

    Bob_orf9	bb3bde1de955af5b7f49a84ba2c4d4ae	369	hamap	MF_00456	Glutamate 5-kinase/delta-1-pyrroline-5-carboxylate synthase	4	259	35.881714	T	28-08-2024	IPR005715	Glu_5kinase/COase_Synthase		
    Bob_orf9	bb3bde1de955af5b7f49a84ba2c4d4ae	369	CDD	cd04242	Glutamate-5-kinase domain	5	255	372.932	T	28-08-2024	IPR041739	G5K_ProB		
    Bob_orf9	bb3bde1de955af5b7f49a84ba2c4d4ae	369	CDD	cd21157	None	263	356	105.626	T	28-08-2024	None	None		
    Wilf_orf50	f927b0d241297dcc9a1c5990b58bf3c4	122	CDD	cd02947	None	15	109	116.118	T	28-08-2024	None	None		
    reverse_orf59	d1b6cbf29dde9e5220196f3f6114a1c3	128	CDD	cd00199	None	76	126	47.4447	T	28-08-2024	None	None		
    ENA|AACH01000027|AACH01000027.2_orf74	fd0743a673ac69fb6e5c67a48f264dd5	449	hamap	MF_00456	Glutamate 5-kinase/delta-1-pyrroline-5-carboxylate synthase	84	339	35.881714	T	28-08-2024	IPR005715	Glu_5kinase/COase_Synthase		
    ENA|AACH01000027|AACH01000027.2_orf74	fd0743a673ac69fb6e5c67a48f264dd5	449	CDD	cd04242	Glutamate-5-kinase domain	85	335	372.546	T	28-08-2024	IPR041739	G5K_ProB		
    ENA|AACH01000027|AACH01000027.2_orf74	fd0743a673ac69fb6e5c67a48f264dd5	449	CDD	cd21157	None	343	436	105.241	T	28-08-2024	None	None		


JavaScript Object Notation (JSON)
---------------------------------

The ``JSON`` output file includes data for all sequences submitted to ``InterProScan``.

Data in the JSON file
~~~~~~~~~~~~~~~~~~~~~

In the ``JSON`` file, each query sequence is represented by its JSON object, that 
contains all match and annotation retrieved and calculated by ``InterProScan``.

The JSON object for each query sequences contains:

For each input/query sequence:

* ``sequence``: The submitted protein or nucleotie sequence
* ``md5``: MD5 hash of the submitted sequence
* ``matches``: List of matches from pre-calculated matches and matches generated by the analysis. Specifically, it is a list of JSON objects, each JSON object representing a match. For each match:
    * ``evalue``: Overall, full sequence evalue
    * ``score``: Overall, full sequence bit-score
    * ``model-ac``: Accession of the member database model
    * ``signature``: A JSON object summarising the InterPro signature
        * ``accession``: Signature accession
        * ``name``: Name from the InterPro entry
        * ``description``: Description from the InterPro entry
        * ``entry``: The accession of the InterPro entry that the signature is associated with
            * entry will be null if a singature is not associated with an InterPro entry
            * ``accession``: The InterPro entry accession
            * ``name``: The InterPro entry name
            * ``description``: The InterPro entry description
            * ``type``: The type of InterPro entry (e.g. family, domain, etc.)
            * ``goXRefs``: Geneontology (GO) terms associated with the InterPro entry - only retrieved if the ``--goterms`` flag is used
            * ``pathwayXRefs``: Pathway information associated with the InterPro entry - only retrieved if the ``--pathways`` flag is used
        * ``signatureLibraryRelease``: JSON object containing:
            * ``library``: Application/member database name
            * ``version``: Release version number
    * ``locations`` : List of locations where the signature matched the protein sequence. Specifically, this is a list of JSON objects, one JSON object per location where a match between the protein sequence and signature was found. For each location:
        * ``start`` Start point of the alignment location with respect to the query sequence -- listed as "**ali** coord **from**" in HMMER
        * ``end`` End point of the alignment location with respect to the query sequence -- listed as "**ali** coord **to**" in HMMER
        * ``hmmStart`` Start point of the local alignment with respect to the HMM profile -- listed as "**hmm** coord **from**" in HMMER
        * ``hmmEnd`` End point of the local alignment with respect to the HMM profile -- listed as "**hmm** coord **to**" in HMMER
        * ``evalue``: Independent E-value
        * ``score``: Bit score
        * ``envelopesStart``: Start of the envelop -- listed as "**env** coord **from**" in HMMER
        * ``envelopeEnd``: End of the envelop -- listed as "**env** coord **to**" in HMMER
        * ``location-fragments``: List of JSON objects, one JSON object per fragment:
            * ``start``: Start location of the fragment in the query sequence
            * ``end``: End location of the fragment in the query sequence
            * ``dc-status``: Continuous/discontinuous status.
        * ``sites``: List of JSON objects, one JSON object per site (a domain signature can have multiple sites). Per site:
            * ``description``: Site description (from InterPro)
            * ``numLocations``: The number of locations (it is the same as the lengh of ``siteLocations`` - so do we need it?)
            * ``label``: Legacy key from ``InterProScan`` version 5
            * ``group``: Legacy key from ``InterProScan`` version 5
            * ``hmmStart``: Legacy key from ``InterProScan`` version 5
            * ``hmmEnd``: Legacy key from ``InterProScan`` version 5 
            * ``siteLocations``: List, one JSON object structure per location:
                * ``start``: Start location of the site in the query sequence
                * ``end``: End location of the site in the query sequence
                * ``residue``: The amino acid residue of the site
    * ``xref``: The protein sequence ID and description listed in the input FASTA file

An example JSON file
~~~~~~~~~~~~~~~~~~~~

Below is a truncated example of the contents of a JSON file. You can recreate the full output 
using the command:

.. code-block:: bash

    nextflow run ebi-pf-team/interproscan6 \
        --input tests/data/test_prot.fa \
        --datadir data/ \
        --disablePrecalc \
        --goterms \
        --pathways \
        -profile docker,local

.. code-block:: JSON

    {
        {
            "sequence": """MDNVNKLTAISLAVAAALPMMASADVMITEYVEGSSNNKAIELYNSGDTAIDLAGYKLVRYKDGATVASD
                MVALDGQSIAPKTTKVILNSSAVITLDQGVDSYSGSLSFNGGDAVALVKDDAVVDIIGDVPTPTGWGFDVTLKRKLDALVANT
                ...
                FERQGSKIEKGYGLINLNTKAHGAGTYSYSYNGELGNLDHALANASLAKRLVDIEDWHINSVESNLFEYGKKFSGDLAKSENA
                FSASDHDPVIVALSYPAPVVPPKPEPTPKDDGGALGYLGLALMSLFGLQRRRR""",
            "md5": "3156952d6b1f52bf18e848ccc4e7e455",
            "matches": [
                {
                    "signature": {
                        "accession": "NF033681",
                        "name": "ExeM_NucH_DNase",
                        "description": "ExeM/NucH family extracellular endonuclease",
                        "signatureLibraryRelease": {
                            "library": "NCBIFAM",
                            "version": "14.0"
                        },
                        "entry": {
                            "accession": "IPR047971",
                            "name": "ExeM-like",
                            "description": "Extracellular endonuclease ExeM-like",
                            "type": "FAMILY",
                            "goXRefs": [],
                            "pathwayXRefs": []
                        }
                    },
                    "locations": [
                        {
                        "start": 221,
                        "end": 831,
                        "representative": false,
                        "evalue": 5.4e-180,
                        "score": 611.4,
                        "hmmStart": 1,
                        "hmmEnd": 545,
                        "hmmLength": 546,
                        "hmmBounds": "N_TERMINAL_COMPLETE",
                        "envelopeStart": 221,
                        "envelopeEnd": 832,
                        "postProcessed": false,
                        "location-fragments": [
                            {
                            "start": 221,
                            "end": 831,
                            "dc-status": "CONTINUOUS"
                            }
                        ]
                        }
                    ],
                    "evalue": 4.5e-180,
                    "score": 611.7,
                    "model-ac": "NF033681"
                },
                {
                    "signature": {
                        "accession": "cd04486",
                        "name": "YhcR_OBF_like",
                        "description": "YhcR_OBF_like",
                        "signatureLibraryRelease": {
                            "library": "CDD",
                            "version": "3.20"
                        },
                        "entry": null
                    },
                    "locations": [
                        {
                        "start": 220,
                        "end": 291,
                        "representative": false,
                        "evalue": 2.23848e-22,
                        "score": 90.0124,
                        "sites": [
                            {
                            "description": "generic binding surface I",
                            "numLocations": 19,
                            "siteLocations": [
                                {
                                "start": "225",
                                "end": "225",
                                "residue": "V"
                                },
                                {
                                "start": "226",
                                "end": "226",
                                "residue": "T"
                                }
                            ]
                            }
                        ],
                        "location-fragments": [
                            {
                            "start": 220,
                            "end": 291,
                            "dc-status": "CONTINUOUS"
                            }
                        ]
                        }
                    ],
                    "model-ac": "cd04486"
                }
            ],
            "xref": [
                {
                "name": "WP_338726824.1 extracellular exonuclease ExeM [Shewanella baltica]",
                "id": "WP_338726824.1"
                }
            ]
        }
    }

An example JSON file (Nucleic)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Below is a truncated example of the contents of a JSON file, generated using nucleic acid 
sequences as input. You can recreate the full output 
using the command:


Extensible Markup Language (XML)
--------------------------------

The richest form of the data is the XML representtaion, and includes data for all sequences 
listed in the input FASTA File.

    nextflow run ebi-pf-team/interproscan6 \
        --input utilities/test_files/test_nt_seqs.fasta \
        --disablePrecalc \
        -profile docker,local \
        --nucleic \
        --applications cdd,hamap

.. code-block:: JSON

    {
    "sequence": "atggcggcggaagaaggcgtggtgattgcgtgccataacaaagatgaatttgatgcgcagatgaccaaagcgaaagaagcgggcaaagtggtgattattgattttaccgcgagctggtgcggcccgtgccgctttattgcgccggtgtttgcggaatatgcgaaaaaatttccgggcgcggtgtttctgaaagtggatgtggatgaactgaaagaagtggcggaaaaatataacgtggaagcgatgccgacctttctgtttattaaagatggcgcggaagcggataaagtggtgggcgcgcgcaaagatgatctgcagaacaccattgtgaaacatgtgggcgcgaccgcggcgagcgcgagcgcg",
    "md5": "e9b174d63adc63bab79c90fdbc8d1670",
    "crossReferences": [
    {
        "name": "Wilf",
        "id": "Wilf"
    }
    ],
    "openReadingFrames": [
        {
          "start": 1,
          "end": 366,
          "strand": "SENSE",
          "protein": {
            "sequence": "MAAEEGVVIACHNKDEFDAQMTKAKEAGKVVIIDFTASWCGPCRFIAPVFAEYAKKFPGAVFLKVDVDELKEVAEKYNVEAMPTFLFIKDGAEADKVVGARKDDLQNTIVKHVGATAASASA",
            "md5": "f927b0d241297dcc9a1c5990b58bf3c4",
            "matches": [
              {
                "signature": {
                  "accession": "cd02947",
                  "name": "TRX_family",
                  "description": "-",
                  "signatureLibraryRelease": {
                    "library": "CDD",
                    "version": "3.20"
                  },
                  "entry": null
                },
                "locations": [
                  {
                    "start": 15,
                    "end": 109,
                    "representative": false,
                    "evalue": 9.55092e-36,
                    "score": 116.118,
                    "sites": [
                      {
                        "description": "catalytic residues",
                        "numLocations": 2,
                        "siteLocations": [
                          {
                            "start": 40,
                            "end": 40,
                            "residue": "C"
                          },
                          {
                            "start": 43,
                            "end": 43,
                            "residue": "C"
                          }
                        ]
                      }
                    ],
                    "location-fragments": [
                      {
                        "start": 15,
                        "end": 109,
                        "dc-status": "CONTINUOUS"
                      }
                    ]
                  }
                ],
                "model-ac": "cd02947"
              }
            ],
            "xref": [
              {
                "name": "orf50 source=Wilf coords=1..366 length=122 frame=1 desc=",
                "id": "orf50"
              }
            ]
          }
        },
    ],
    }

Data in the XML file
~~~~~~~~~~~~~~~~~~~~

The XML Schema Definition (XSD) is available
`here <http://ftp.ebi.ac.uk/pub/software/unix/iprscan/5/schemas/>`_. ``InterProScan6`` uses the 
latest XSD.

For each query sequence:

* ``sequence``: The submitted protein or nucleotie sequence
* ``xref``: The sequence ID and name/description from the input FASTA file
* ``md5``: MD5 hash of the submitted sequence
* ``matches``: List of matches from pre-calculated matches and matches generated by the analysis.
    * ``hmmer3-match``: Represents a HMMER3 match: AntiFam, NCBIFam, FunFam, Gene3D, HAMAP, Panther, SFLD, SUPERFAMILY
    * ``<member-name>-match``: Match from a member database that does not use HMMER, e.g. CDD
    * The information for both these keys is very similar and is summarised here:
        * ``signature``: Represents the member database signature. Includes accession, name and description
            * ``entry``: Associated InterPro entry. Includes entry accession, description, name and type (e.g. family, domain, etc.), as well as any associated pathway information (if the ``--pathway`` flag is used) and Geneontology (GO) terms (of the ``--goterms`` flag is used)
            * ``library release``: Release version of the member datbase. Includes name and version/release number
        * ``models``: Information about the model, including the name, and accession
        * ``locations``: Represents domain hits in the query sequence. Includes:
            * E-value
            * Score: The bitscore or other member database relevant score
            * The envelop start and end: Start and end point of the envelop - listed as "env to/from" in HMMER
            * Hmm-start and hmm-end: Start and end point of the local alignemnt with respect to the HMM profile - listed as "hmm to/from" in HMMER
            * Hmm-length: Length of the alignemnt along the query sequence
            * Hmm-bounds: Description of the HMMER Hmm bound pattern
            * start and end: Start and end point of the alignment location with respect to the query sequence - listed as "ali to/from" in HMMER
            * alignemnt: The query sequence alignment to the model
            * cigar-alignemnt: The `cigar alignment <https://replicongenetics.com/cigar-strings-explained/>`_
            * ``site-loctaions``: information about sites (for those member databases that contain site data):
                * Each site is represented by a ``site-location``, which as a start, stop and residue.

**HMM Bounds:** (Quoted from the  official `HMMER3 documentation <http://eddylab.org/software/hmmer/Userguide.pdf>`_):
It’s not immediately easy to tell from the “to” coordinate whether
the alignment ended internally in the query or target, versus ran
all the way (as in a full-length global alignment) to the end(s). To
make this more readily apparent, with each pair of query and target
endpoint coordinates, there’s also a little symbology: ``..`` means both
ends of the alignment ended internally, ``[]`` means both ends of the
alignment were full-length flush to the ends of the query or target,
and ``[.`` and ``.]`` mean only the left or right end was flush/full length.

.. TIP::
    To understand the meaning of the values in the ``InterProScan`` output for member databases 
    that implement ``hmmsearch`` from HMMER3, we recommed referring to the offical 
    `HMMER3 documentation <http://eddylab.org/software/hmmer/Userguide.pdf>`_, specifically 
    under the "Step 2: search the sequence database with hmmsearch" header.

An example XML file
~~~~~~~~~~~~~~~~~~~

Below is an extract from an ``InterProScan`` output XML file. You can recreate the full output 
using the command:

.. code-block:: bash

    nextflow run ebi-pf-team/interproscan6 \
        -profile docker,local \
        --input tests/data/test_prot.fa \
        --datadir data/ \
        --disablePrecalc \
        --goterms \
        --pathways

Below is an extract from a XML output file, showing the results for one protein:

.. code-block:: xml

    <protein>
    <sequence md5="268e4659f70d6eb10e6545eccaa347cf">MLIERMFPFISESVRVHQLPEGGVLEIDYMRDNVSISDFEYLDLNKTAYELCMLMDGQKTAEQILEYQCAAYNESPEDHKDWYYEMLDMLLNKQVIRLTDQPEYRRIATSGSSDFPMPLHATFELTHRCNLKCAHCYLESSPEALGTVSLEQFKKTADMLYEKGVLTCEITGGEIFVHPNANELLEYVLKKFKKVAVLTNGTLMRKESLEILRAYKQKIIVGISLDSVHSEVHDSFRGRKGSFAQTCKTIKLLSDHGIFVRVAMSVFEKNMWEIHDMAQKVRDLGAKAFSYNWVDDFGRGKDMIHPTKDAEQHRKFMEYEQNVIDEFKDLIPIIPYERKRAANCGAGWKSIVISPFGEVRPCALFPKEFSLGNIFHDSYESIFDSALVHKLWKAQAPRFSEHCKKDKCPFSGYCGGCYLKGLNSNKYHRKNICSWAKNEQLEDVVQLI</sequence>
    <xref id="A0A0H3E4R3_BACA1" name="A0A0H3E4R3_BACA1 1-448"/>
    <matches>
    <hmmer3-match evalue="0.0" score="609.7">
        <signature ac="SFLDG01386" desc="main SPASM domain-containing" name="main_SPASM_domain-containing">
        <signature-library-release library="SFLD" version="4"/>
        </signature>
        <model-ac>SFLDG01386</model-ac>
        <locations>
            <hmmer3-location env-end="448" env-start="1" score="609.6" evalue="0.0" hmm-start="1" hmm-end="443" hmm-length="349" hmm-bounds="N_TERMINAL_COMPLETE" start="1" end="447" representative="false">
            <location-fragments>
            <hmmer3-location-fragment start="1" end="447" dc-status="CONTINUOUS"/>
            </location-fragments>
            <sites>
                <hmmer3-site description="Binds [4Fe-4S]-AdoMet cluster" numLocations="3">
                <site-locations>
                <site-location residue="C" start="129" end="129"/>
                <site-location residue="C" start="136" end="136"/>
                <site-location residue="C" start="133" end="133"/>
                </site-locations>
                <group>0</group>
                <hmmEnd>0</hmmEnd>
                <hmmStart>0</hmmStart>
                </hmmer3-site>
            </sites>
            </hmmer3-location>
        </locations>
    </hmmer3-match>
    <hmmer3-match evalue="0.0" score="609.7">
        <signature ac="SFLDF00315" desc="antilisterial bacteriocin subtilosin biosynthesis protein (AlbA-like)" name="antilisterial_bacteriocin_sub">
        <signature-library-release library="SFLD" version="4"/>
        </signature>
        <model-ac>SFLDF00315</model-ac>
        <locations>
            <hmmer3-location env-end="448" env-start="1" score="609.6" evalue="0.0" hmm-start="1" hmm-end="443" hmm-length="448" hmm-bounds="N_TERMINAL_COMPLETE" start="1" end="447" representative="false">
            <location-fragments>
            <hmmer3-location-fragment start="1" end="447" dc-status="CONTINUOUS"/>
            </location-fragments>
            <sites>
                <hmmer3-site description="Binds [4Fe-4S]-AdoMet cluster" numLocations="3">
                <site-locations>
                <site-location residue="C" start="129" end="129"/>
                <site-location residue="C" start="136" end="136"/>
                <site-location residue="C" start="133" end="133"/>
                </site-locations>
                <group>0</group>
                <hmmEnd>0</hmmEnd>
                <hmmStart>0</hmmStart>
                </hmmer3-site>
            </sites>
            </hmmer3-location>
        </locations>
        </hmmer3-match>
    </matches>
    </protein>

An example XML file (Nucleic)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Below is an extract from an ``InterProScan`` output XML file, generated when the input FASTA file contained 
nucleic acid sequences. You can recreate the full output 
using the command:

.. code-block:: bash

    nextflow run ebi-pf-team/interproscan6 \
        -profile docker,local
        --input tests/data/test_prot.fa \
        --disablePrecalc \
        --goterms \
        --pathways

Below is an extract from a XML output file, showing the results for one protein:

.. code-block:: xml

	<nucleotide-sequence>
		<sequence md5="e9b174d63adc63bab79c90fdbc8d1670">atggcggcggaagaaggcgtggtgattgcgtgccataacaaagatgaatttgatgcgcagatgaccaaagcgaaagaagcgggcaaagtggtgattattgattttaccgcgagctggtgcggcccgtgccgctttattgcgccggtgtttgcggaatatgcgaaaaaatttccgggcgcggtgtttctgaaagtggatgtggatgaactgaaagaagtggcggaaaaatataacgtggaagcgatgccgacctttctgtttattaaagatggcgcggaagcggataaagtggtgggcgcgcgcaaagatgatctgcagaacaccattgtgaaacatgtgggcgcgaccgcggcgagcgcgagcgcg</sequence>
		<xref id="Wilf" name="Wilf" />
    		<orf end="366" start="1" strand="SENSE">
                <protein>
                    <sequence md5="f927b0d241297dcc9a1c5990b58bf3c4">MAAEEGVVIACHNKDEFDAQMTKAKEAGKVVIIDFTASWCGPCRFIAPVFAEYAKKFPGAVFLKVDVDELKEVAEKYNVEAMPTFLFIKDGAEADKVVGARKDDLQNTIVKHVGATAASASA</sequence>
                    <xref id="orf50" name="orf50 source=Wilf coords=1..366 length=122 frame=1 desc=" />
                    <matches>
                        <cdd-domain>
                            <signature ac="cd02947" desc="" name="">
                                <entry ac="-" desc="" name="" type="Domain" />
                                <signature-library-release library="CDD" version="3.20" />
                            </signature>
                            <model-ac>cd02947</model-ac>
                            <locations>
                                <analysis-location end="109" start="15" representative="" evalue="9.55092e-36" score="116.118">
                                    <location-fragments>
                                        <analysis-location-fragment description="catalytic residues" numLocations="2" />
                                    </location-fragments>
                                </analysis-location>
                            </locations>
                        </cdd-domain>
                    </matches>
                </protein>
		    </orf>
	</nucleotide-sequence>
