========================================
How InterProScan operates under the hood
========================================

Each member database or application in ``InterProScan`` has its own unique method of analysis. 
Below we outline the method of analysis for each member database.

Applications and member databases
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

We divide the member databases into two groups: those that use HMMER and those that don't and instead
use a member-database-specific tool.

* `HMMER`_
   * `AntiFam`_
   * `Cath-Gene-3D`_
   * `FunFam`_
   * `HAMAP`_
   * `NCBIFam`_
   * `Panther`_
   * `Pfam`_
   * `PirsF`_
   * `PirsR`_
   * `SFLD`_
   * `SMART`_
   * `Superfamily`_
* `Member db specific tools`_
   * `CDD`_
   * `Coils`_
   * `MobiDB`_
   * `Phobius`_
   * `PRINTS`_
   * `Prosite Patterns`_
   * `Prosite Profiles`_
   * `SignalP`_
   * `(Deep)TMHMM`_

Third party tools
~~~~~~~~~~~~~~~~~

- `HMMER <https://academic.oup.com/nar/article-lookup/doi/10.1093/nar/gky448>`__- Assesses alignment between query sequence and Hidden Markov Models (HMMs)
- `cath-resolve-hits <https://doi.org/10.1093/bioinformatics/bty863>`__- helps to resolve domain matches
- `TreeGrafter <https://doi.org/10.1093/bioinformatics/bty625>`__- a tool for annotating uncharacterised protein sequences, using annotated phylogenetic trees.
- `rpsblast <https://www.animalgenome.org/blast/doc/rpsblast.html>`__- reversed position specific BLAST.
- `nCoils <https://doi.org/10.1016/S0076-6879(96)66032-7>`__- tool for the prediction and analysis of coiled-coil structures
- `modidb <https://doi.org/10.1093/nar/gkac1065>`__- Database and software for intrinsically disordered proteins
- `phobius <https://doi.org/10.1016/j.jmb.2004.03.016>`__- Tool for prediction of transmembrane domains
- `fingerPRINTScan <https://doi.org/10.1093/bioinformatics/15.10.799>`__- Search against FingerPRINTScan with a protein query sequence to identify the closest matching PRINTS sequence motif fingerprints in a protein sequence.
- `pfsearchV3 <https://doi.org/10.1093/bioinformatics/btt129>`__- a code acceleration and heuristic to search PROSITE profiles
- `signalP <https://www.nature.com/articles/s41587-021-01156-3>`__- predict signal peptides
- `TmHMM <https://doi.org/10.1006/jmbi.2000.4315>`__- a tool to predict transmembrane domains

Member databases that use HMMER
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

This section lists all the tools whose primary analysis is completed using ``HMMER``.

Each member databases requires its own HMMs.

* `AntiFam`_
* `Cath-Gene-3D`_
* `FunFam`_
* `HAMAP`_
* `NCBIFam`_
* `Panther`_
* `Pfam`_
* `PirsF`_
* `PirsR`_
* `SFLD`_
* `SMART`_
* `Superfamily`_

Note, the names of the modules listed below are written as they are presented in the termianl 
when running ``InterProScan``. Thus Cath-Gene3D is listed as Gene3D and MobiDB-Lite is listed as MobiDB.

AntiFam
-------

AntiFam is a resource of profile-HMMs designed to identify spurious protein predictions, 
specifically HMMs that represent protein sequence families from spurious open reading frames (ORFs).

Annotation of AntiFam domains requires only implementing HMMER, and requires no post-processing 
of the HMMER hits.

1. ``ANTIFAM_HMMER_RUNNER``: Protein sequences are analysed using HMMER3 and the HMM profiles in the 
    AntiFam HMM file (``data/antifam/version/AntiFam.hmm``)
2. ``ANTIFAM_HMMER_PARSER``: The ``HMMER.out`` file is parsed using ``GENERIC_HMMER_PARSER`` process in 
    IPS6, parsing the data into the internal IPS6 JSON structure.

**What is AntiFam?:** During the lifetime of the Pfam protein families database a number of protein 
families have been built, which were later identified as composed solely of spurious open 
reading frames (ORFs) either on the opposite strand or in a different, overlapping reading 
frame with respect to the true protein-coding or non-coding RNA gene. These families were 
deleted and are no longer available in Pfam. These families have been collected, along with 
custom-made families of spurious ORFs, into the AntiFam database where each family is 
represented as a HMM.

FunFam
------

The FunFam (Functional Families) database provides protein function annotations for protein 
families and superfamilies, based upon their evolutionary relationships. Families are derived 
from various sources, including Pfam, CATH, and other structural and sequence-based classifications.
Function annotations are organised into hierarchical categories, defining relationships between 
different functional terms and their associated protein families.

1. ``GENE3D_HMMER_RUNNER``: Protein sequences are analysed using HMMER3 and the HMM profiles in the 
Gene3D HMM file (``data/gene3d/version/gene3d_main.hmm``)
2. ``GENE3D_HMMER_PARSER``: The ``HMMER.out`` file is parsed into the internal ``IPS6`` JSON structure.
3. ``CATH_RESEOLVE_HITS``: The ``HMMER.out`` output file is parsed by ``cath-resolve-hits`` to 
collapse lists of domain matches to the best, non-overlapping subset.
4. ``GENE3DADD_CATH_SUPERFAMILIES``: A Python script provided by the Gene3D maintainers, 
``assign_cath_superfamilies.py`` is used to parse the output (the filtered domain matches) from ``cath-resolve-hits`` and assign the Cath 
superfamily models and associated metadata to the domain matches (based upon the domain ID's), 
adding these data to the internal IPS6 JSON structure and writing out a plain text file of FunFam 
HMM models to use.
5. ``FUNFAM_HMMER_RUNNER``: The protein sequences are analysed using HMMER3 and the relevant HMM profiles for the FunFam 
HMM, by parsing the file from ``assign_cath_superfamilies.py``
6. ``FUNFAM_CATH_RESEOLVE_HITS``: The ``HMMER.out`` output file is parsed by ``cath-resolve-hits`` to 
collapse lists of domain matches to the best, non-overlapping subset.
7. ``FUNFAM_ADD_CATH_SUPERFAMILIES``: The python script 
``assign_cath_superfamilies.py`` is used again to parse the output (the filtered domain matches) from
``cath-resolve-hits`` and assign the Cath superfamily models and associated metadata to the domain matches (based upon the domain ID's).
8. ``FUNFAM_PARSER``: An in-house python script parses the output from 
``assign_cath_superfamilies.py``, adding these data into the internal ``IPS6`` JSON structure.

FunFam needs Cath-Gene3d
^^^^^^^^^^^^^^^^^^^^^^^^

FunFam requires Cath-Gene3D to also be executed as well as 
the hits from Cath-Gene3D to be filtered using `cath-resolve-hits`. This is because the 
FunFam database contains an exorbitant number of HMM profiles; running a set of query 
sequences against the entire FunFam database would, therefore, take an very long time. 
FunFam families are derived from Cath-Gene3D superfamilies. Therefore, the query protein 
sequences are screened against the Cath-Gene3D database to identify which Cath-Gene3D 
superfamilies are present in the input dataset. The query sequences are then only screened 
against the FunFam families that belong to the matched Cath-Gene3D superfamilies, thus 
running the query sequences against a subset of FunFam families.

Gene3D
------

The Cath-Gene3D database, or Gene3D, is a resource that provides structural annotation and 
evolutionary relationships for protein domains.

Cath-Gene3D uses the ``cath-resolve-hits`` `tool <https://cath-tools.readthedocs.io/en/latest/tools/cath-resolve-hits/>`_ 
from the ``cath-tools`` suite to collapse a list of domain matches to the query sequence(s) 
down to the best, non-overlapping subset (ie domain architecture), with the aim to minimise 
suprious matches.

1. ``GENE3D_HMMER_RUNNER``: Protein sequences are analysed using HMMER3 and the HMM profiles 
in the Gene3D HMM file (``data/gene3d/version/gene3d_main.hmm``)
2. ``GENE3D_HMMER_PARSER``: The ``HMMER.out`` file is parsed into the internal ``IPS6`` JSON structure.
3. ``GENE3D_CATH_RESEOLVE_HITS``: The ``HMMER.out`` output file is parsed by ``cath-resolve-hits`` 
    to collapse lists of domain matches to the best, non-overlapping subset.
4. ``GENE3D_ADD_CATH_SUPERFAMILIES``: A third-party (Cath-Gene3D) python script 
    ``assign_cath_superfamilies.py`` is used to parse the output (the filtered domain matches) from 
    ``cath-resolve-hits`` and assign the Cath superfamily models and associated metadata to the 
    domain matches (based upon the domain ID's).
5. ``GENE3D_FUNFAM_PARSER``: An in-house python script parses the output from 
    ``assign_cath_superfamilies.py``, adding these data into the internal ``IPS6`` JSON structure.

CATH
^^^^

* Class - Defined by the secondary structure composition (e.g., mainly alpha, mainly beta, alpha and beta, etc.)
* Architecture - Defined by the overall shape formed by the secondary structures.
* Topology - Defined by the connectivity and orientation of the secondary structures.
* Homologous Superfamily - Clusters of domains thought to share an evolutionary ancestor, and thus may be functionally related.

Gene3D
^^^^^^

Predicts the occurence of domain families in protein sequences, identifying and modeling domain 
families based upon sequence similarirty.

HAMAP
-----

The HAMAP (High-quality Automated and Manual Annotation of Microbial Proteomes) database 
provides high-quality, automated annotation of microbial proteins, focusing on bacteria, 
archeae and plastids. HAMAP classifies proteins into families based on sequence similarity 
and functional characteristics.

1. ``HAMAP_HMMER_RUNNER``: Protein sequences are analysed using HMMER3 and the HMM profiles in the 
HAMAP HMM file (``data/hamap/version/hamap.hmm``)
2. ``HAMAP_HMMER_PARSER``: The ``HMMER.out`` file is parsed into the internal ``IPS6`` JSON structure.
3. ``HAMAP_PFSEARCH_RUNNER``: The protein sequences are analaysed by the PROSITE ``pfsearchV3`` 
tool, coordinated by the PROSITE ``pfsearch_wrapper.py`` script, using the HAMAP profile models 
(``data/hamap/version/profiles/``).
4. ``HAMAP_PARSER``: Parse the output from ``pfsearch`` into the ``IPS6`` JSON structure, 
additionally filtering the results selected by ``pfsearchV3`` and filtering to results with 
the pass level of 'ONE' or 'ZERO'.

NCBIFam
-------

NCBIfam is a collection of protein families based on Hidden Markov Models (HMMs). NCBIFam is 
part of the NCBI's collection of Protein Family Modules. It includes HMM models built from 
scratch by NCBI curators and models derived from a curated collection of protein clusters.

Annotation of NCBIFam domains requires only implementing HMMER, and requires no post-processing 
of the HMMER hits.

1. ``NCBIFAM_HMMER_RUNNER``: Protein sequences are analysed using HMMER3 and the HMM profiles in the 
NCBIFam HMM file (``data/ncbifam/version/NcbiFam.hmm``)
2. ``NCBIFAM_HMMER_PARSER``: The ``HMMER.out`` file is parsed, parsing the data into the internal 
``IPS6`` JSON structure.

Panther
-------

The PANTHER (Protein Analysis Through Evolutionary Relationships) database is a comprehensive resource that provides evoltionary and functional information about protein-coding genes, organising protein sequences into families of homologous genes. It classifies genes by their functions, using published scientific experimental evidence and evolutionary relationships to predict function even in the absence of direct experimental evidence.

1. ``PANTHER_HMMER_RUNNER``: Protein sequences are analysed using HMMER3 and the HMM profiles 
in the Panther HMM file (``data/panther/version/famhmm/panther_hmm``)
2. ``PANTHER_HMMER_PARSER``: The ``HMMER.out`` files is parsed into the internal ``IPS6`` 
JSON structure.
3. ``PANTHER_POST_PROCESSER``: The input protein sequences and the hits from HMMER are 
parsed to identify the 
the best matching homologous family. This means there is only ever a maximum of one domain 
hit for a Panther signature within a protein.  The Python package ``TreeGrafter`` is then 
implemented, whcich uses the +15,000 phylogenetic trees in Panther to identify the best 
location of each HMMER hit in the tree. This is used to infer PANTHER sunfamiy annotations, 
and PAINT annotations.
4. ``PANTHER_PARSER``: The output from ``TreeGrafter`` is added to the internal ``IPS6`` 
JSON by the in-house Python script ``process_treegrafter_hits.py``

Only one match per protein
^^^^^^^^^^^^^^^^^^^^^^^^^^

Panther (through the use of ``TreeGrafter``) only takes the best match for each protein sequence, thus only producing **one** match per sequence. This means that in the output JSON file, the E-value and score and not contained under the ``locations`` key, but instead under the ``signature`` key.

TreeGrafter
^^^^^^^^^^^

``TreeGrafter`` is a Python package that looks for the best matching homologous family in a library of pre-calculated, pre-annotated gene trees, grafting the the input sequence to the best location in the tree. The sequence is then annotated by propagating annotations from ancestral nodes in the reference tree. It only allows **one** (the best) match per protein sequence.

PAINT Annotations
^^^^^^^^^^^^^^^^^

PAINT (Phylogenetic Annotation and Inference Tool) annotations are a part of the PANTHER (Protein Analysis THrough Evolutionary Relationships) system. They are used to capture inferences about the evolution of gene function within a gene family, including the gain, inheritance, modification, and loss of function over evolutionary time.

Pfam
----

Pfam is a comprehensive database of protein families and domains. It is a collection of multiple sequence alignments and hidden Markov models (HMMs) representing protein domains and families. 

1. ``PFAM_HMMER_RUNNER``: Protein sequences are analysed using HMMER3 and the HMM profiles 
in the Pfam HMM file (``data/pfam/version/pfam_a.hmm``)
2. ``PFAM_PARSER``: The hits from HMMER are parsed by an in-house post-processing script which 
   for each match decides if to keep or ignore the match by comparing the current match 
   to previously evaluated Pfam matches (which we decided to keep). A match is ignored when: 
   the match overlaps another match, both matches belong to the same clan, and one of the matches 
   is nested in the other. The script parses the selected matches into the internal IPS6 JSON structure.

Nested domains
^^^^^^^^^^^^^^

When evaluating two domains to see if one is nested in the other, the parents of each domain 
are also be considered. Let's say you have two overlapping domains, PFXXXXX and PFYYYYY that 
belong to the same clan. Let's also say that PFXXXXX is not nested in PFYYYYY and PFYYYYY is not 
nested in PFXXXXX. But maybe PFXXXXX is nested in PFZZZZZ and  PFZZZZZ is nested in PFYYYYY.

PirsF
-----

The Protein Information Resource SuperFamily (PISRF) database provides a classification of 
protein sequences into superfamilies based on whole-protein sequence similarity. PIRSF groups 
proteins into hierarchical clusters, ranging from broad superfamilies to more specific subfamilies.

The PIRSF concept is used as a guiding principle to provide comprehensive and non-overlapping 
clustering of UniProtKB sequences into a hierarchical order to reflect their evolutionary relationships.

1. ``PIRSF_HMMER_RUNNER``: Protein sequences are analysed using HMMER3 and the HMM profiles in the 
    PirsF HMM file (``data/pirsf/version/pirsf.hmm``).
2. ``PIRSF_HMMER_PARSER``: The ``HMMER.out`` file is parsed into the internal ``IPS6`` JSON structure.
3. ``PIRSF_RUNNER``: The PirsF perl script ``pirsf.pl`` is used to post-process HMMER hits in the 
HMMER3 out file.
4. ``PIRSF_PARSER``: The output from ``pirsf.pl`` is parsed, and the additional data is added 
to the internal ``IPS6`` JSON structure, and the hits in the ``IPS6`` JSON are filtered to only 
retain the 'best' family and subfamily matches selected by ``pirsf.pl``.

PirsF vs. PirsR
^^^^^^^^^^^^^^^

PIRSF focuses on classifying entire protein sequences into superfamilies to study functional 
and evolutionary relationships, while PIRSR focuses on annotating specific functional sites 
within protein sequences to provide detailed functional insights.

PirsR
-----

The Protein Information Resource Site Rule (PIRSR) database provides site-specific annotations for proteins, identifying functionally important sites, such as active sites, binding sites, and post-translational modification sites. It is a database of protein families based on hidden Markov models (HMMs) and Site Rules.

1. ``PIRSR_HMMER_RUNNER``: Protein sequences are analysed using HMMER3 and the HMM profiles in the 
PirsR HMM file (``data/pirsr/version/pirsr.hmm``).
2. ``PIRSR_HMMER_PARSER``: The ``HMMER.out`` file is parsed into the internal ``IPS6`` JSON structure.
3. ``PIRSR_RUNNER``: The PirsR python script ``pirsr.py`` is used to post-process HMMER hits in 
the HMMER3 ``.dtbl`` file.
4. ``PIRSR_PARSER``: The output from ``pirsr.py`` is parsed, and the additional data is added 
to the internal ``IPS6`` JSON structure, and the hits in the ``IPS6`` JSON.

PirsF vs. PirsR
^^^^^^^^^^^^^^^

PIRSF focuses on classifying entire protein sequences into superfamilies to study functional 
and evolutionary relationships, while PIRSR focuses on annotating specific functional sites 
within protein sequences to provide detailed functional insights.

SFLD
----

The Structure-Function Linkage Database (SFLD) describes structure-function relationships for functionally diverse enzyme superfamilies. SFLD provides a hierarchical classification of enzymes that relates specific sequence-structure features to chemical capabilities, classifying evolutionarily related protein sequences according to shared biochemical functions and mapping these shared functions to conserved active site features.

1. ``SFLD_HMMER_RUNNER``: Protein sequences are analysed using HMMER3 and the HMM profiles 
in the SFLD HMM file (``data/sfld/version/sfld.hmm``). HMMER generates a ``HMMER.out`` file, 
a ``HMMER.dtbl`` file, as well as an alignment file (all three are required for post-processing).
2. ``SFLD_HMMER_PARSER``: The ``HMMER.out`` file is parsed into the internal ``IPS6`` JSON structure.
3. ``SFLD_POST_PROCESSER``: The hits from HMMER are parsed by an in-house post-processing 
script (a binary file compiled from ``sfld_postprocess.h`` and ``sfld_postprocess.c``) which 
parses the ``HMMER.out``, ``HMMER.dtbl`` and alignment file from HMMER. This post-processing 
filteres the matches to only retain domains where all sites (from InterPro) match between the 
model and the query protein sequence, as well as add site annotation data. The output is written 
in the ``HMMER.dtbl`` format.
4. ``SFLD_PARSER``: An in-house Python script parses the output from ``sfld_postprocess``, 
filtering the matches in and adding SFLD site data to the internal IPS6 JSON architecture.

Hierarchical classification in SFLD
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

**Family:** A set of evolutionarily related enzymes that catalyze the same overall reaction.

**Superfamily:** A broader set of evolutionarily related enzymes with a shared chemical function that maps to a conserved set of active site features.

**Functional Domain:** A single member of a family, either a whole protein or the domains responsible for the enzymatic activity.

**Subgroup:** A set of evolutionarily related enzymes that have more shared features than the superfamily as a whole, but may still catalyze different overall reactions.

SMART
-----

The SMART (Simple Modular Architecture Research Tool) is a web resource that allows the 
identification and annotation of genetically mobile domains and the analysis of domain architectures. 
These domain are extensively annotated with respect to phyletic distributions, functional class, 
tertiary structures and functionally important residues.

1. ``SMART_HMMER_RUNNER``: Protein sequences are analysed using HMMER3 and the HMM profiles in 
the SMART HMM file (``data/smart/version/smart.hmm``).
2. ``SMART_HMMER_PARSER``: The ``HMMER.out`` file is parsed into the internal ``IPS6`` JSON structure.

``InterProScan`` by default uses the implementation of SMART that contains no licensed components. 
Post-processing of SMART matches requires 2 licensed files that need to be obtained from 
SMART for threshold and overlap data. The licensed "overlapping" and "THRESHOLDS" files 
are not included with an ``InterProScan`` by default, and are therefore, not used to post-process
the SMART matches.

SUPERFAMILY
-----------

SUPERFAMILY is a database of structural and functional annotations for all proteins and genomes, and aids classifying protein sequences into structural and functional superfamilies based on their structural domains. SUPERFAMILY uses HMMs to detect structural domains within protein sequences.

1. ``SUPERFAMILY_HMMER_RUNNER``: Protein sequences are analysed using HMMER3 and the HMM 
profiles in the SUPERFAMILY HMM file (``data/superfamily/version/superfamily.hmm``).
2. ``SUPERFAMILY_HMMER_PARSER``: The ``HMMER.out`` file is parsed into the internal ``IPS6`` 
JSON structure.
3. ``SUPERFAMILY_POSTPROCESSER``: Run the SUPERFAMILY perl script ``ass3_single_threaded.pl``.
4. ``SUPERFAMILY_PARSER``: An in-house Python script that parses the binary output from 
``ass3_single_threaded.pl``, filtering the matches in the internal ``IPS6`` JSON and added 
data from the binary to the ``IPS6`` JSON.

Member databases with specific tools
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

This section discusses member databases where their tool is unqiue to them

* `CDD`_
* `Coils`_
* `MobiDB`_
* `Phobius`_
* `PRINTS`_
* `Prosite Patterns`_
* `Prosite Profiles`_
* `SignalP`_
* `(Deep)TMHMM`_

CDD
---

The ``CCD: Conserved Domain Database`` is a bioinformatic resource from NCBI that provides 
information about domains that are conserved across multiple different species. Specifically, 
CDD contains a collection of well-annotated multiple sequence alignment (MSA) models (HMMs), 
representing ancient domains and full-length proteins within its database. Models that provide 
significant overlapping annotations are clustered into protein domain superfamilies.

1. ``CDD_RUNNER``: The protein sequences are analysed using ``RPS-BLAST`` from the NCBI ``BLAST+`` 
suite.
2. ``CDD_POSTPROCESS``: The ``rpsbproc`` utility from CDD is used to post-process the hits from 
``RPS-BLAST``.
3. ``CDD_PARSER``: An in-house python script ``cdd_parser.py`` parses the output from ``rpsbproc``, 
filtering hits to only retains those with a hit type of "specific" (thus dropping "non-specific" hits) 
and parsing the output into the internal IPS6 JSON structure.

RPS-BLAST
^^^^^^^^^

Reverse Position-Specific BLAST (``RPS-BLAST``) is a variant of BLAST (the Basic Local Alignment 
Search Tool). ``RPS-BLAST`` searches a query sequence against a database of profiles (instead of a 
database of sequences as with traditional BLAST methods). ``RPS-BLAST`` matches the query sequence 
with a set of conserved domains, Hidden Markov Models (HMMs), or pre-algined profiles.

rspbproc
^^^^^^^^

A wrapper for RPS-BLAST in order to provide results that match those computed by NCBI's on-line 
search services, including site annotation and the location of conserved domain superfamily 
footprints. It is downloaded from the `CDD ftp server <https://ftp.ncbi.nih.gov/pub/mmdb/cdd/rpsbproc/>`_ 
within the IPS6-CDD docker image.

Coils
-----

The Coils database and the accompanying tool ``ncoils`` are used for the identification of coiled-coil motifs in protein sequences.

1. ``COILS_RUNNER``: The protein sequences are analysed using ``ncoils``.
2. ``COILS_PARSER``: An in-house python script ``coils_parser.py`` parses the output from ``ncoils``, parsing the output into the internal IPS6 JSON structure.

* **Coils:** The Coils database is a curated collection of protein sequences that contain coiled-coil motifs.
* **``ncoils:``:** The ncoils tool is a software program designed to predict the presence of coiled-coil motifs in protein sequences. It uses algorithms that compare the input sequence against the Coils database and apply pattern recognition techniques to identify regions likely to form coiled-coil structures.
* **Coiled-coils:** Structural motifs in proteins that are characterised by two or more alpha-helices coiled together. These are often important for protein-protein interactions and the formation of protein complexes.

MobiDB
------

MobiDB and MobiDB-Lite are resources that are focused on the annotation and study of protein disorder and mobility.

1. ``MOBI_RUNNER``: The protein sequences are analysed using ``mobiDB Lite`` binary (packaged into the ``IPS6`` docker container).
2. ``MOBI_PARSER``: An in-house python script ``mobi_parser.py`` parses the output from ``mobiDB``, parsing the output into the internal IPS6 JSON structure.

* **mobiDB:** A comprehensive database of detailed annotations of protein disorder and related features, integrating data from various sources.
* **mobiDB Lite:** A streamlined, simplified version of the mobiDB database, designed for the quick and easy access to information about protein disorder. It provides annotations of disordered regions in proteins, which are segments that do not adopt a fixed three-dimensional structure. This lightweight version is particularly useful for researchers who need rapid access to disorder annotations without the detailed features of the full database.

Phobius
-------

``Phobius`` is a bioinformatic tool for the prediction of signal peptides and transmembrane domains in protein sequences.

1. ``PHOBIUS_RUNNER``: The protein sequences are parsed using the sequence analysis tool ``Phobius`` to predict the presence of signal peptides and transmembrane domains.
2. ``PHOBIUS_PARSER``: The output from ``Phobius`` is parsed into the internal IPS6 JSON structure, using an in-house python script.

* **Signal peptides:** A signal peptide, also known as a signal sequence, localisation sequence, or leader peptide, is a short peptide (protein sequence) that is usually 16-30 amino acids long. It is present at the N-terminus (or occasionally at the C-terminus or internally) of most newly synthesised proteins that are destined toward the secretory pathway. The role of the signal peptide is to prompt the transportation of the protein to a specific region of the cell, often the cell membrane. The signal peptide is typically cleaved following the succcessfully translocation of the protein.
* **Transmembrane regions:** The transmembrane region/domain in a protein sequence is the region of the protein that spans the entirety of the cell membrane. Transmembrane regions are typically composed of hydrophobic (water repelling) amino acids, forming a structure that is compatible with the hydrophobic environment between the lipid bilayers of the cell membrane.

PRINTS
------

The PRINTS database contains conserved motifs (fingerprints) representing protein families, and the ``fingerPRINTScan`` tool is used to identify these motifs in protein sequences, aiding in protein classification and functional prediction.

1. ``PRINTS_RUNNER``: The protein sequences are parsed using the sequence analysis tool ``fingerPRINTScan`` to predict the presence of conserved motifs.
2. ``PRINTS_PARSER``: The output from ``fingerPRINTScan`` is parsed into the internal IPS6 JSON structure, using an in-house python script.

* **PRINT:** The PRINTS database is a collection of protein fingerprints, which are groups of conserved motifs or patterns that characterise protein families. These fingerprints are derived from sequence alignments and are used to identify and classify proteins based on their evolutionary relationships and functional similarities._
* **fingerprint:** A fingerprint is a group of conserved motifs used to characterise a protein family.
* **``fingerPRINTScan``:** fingerPRINTScan is a software tool designed to scan protein sequences for the presence of fingerprints stored in the PRINTS database.

PROSITE Patterns
----------------

PROSITE is a database of protein domains, families, and functional sites. It contains biologically significant sites and patterns that help in identifying these features in protein sequences. PROSITE is widely used for protein annotation and to predict the function of newly discovered proteins based on their sequence similarity to known patterns.

1. ``PROSITE_RUNNER``: The protein sequences are analaysed by the PROSITE perl script ``ps_scan.pl``, using the PROSITE Patterns models (``prosite_patterns.dat``) and evaluator models (``evaluator.dat``), by coordinating running ``pfscanV3``.
2. ``PROSITE_PARSER``: The output from ``ps_scan.pl`` is parsed into the internal ``IPS6`` JSON structure by an in-house Python script, which filteres out all matches that do not have a match level of 'STRONG'.

* **PROSITE Patterns:** PROSITE patterns, also known as motifs or signatures, are short, descriptive sequences that represent conserved regions within protein families. These patterns are typically made up of specific amino acids that are highly conserved and are often critical for the protein's function or structure. Patterns are usually represented using regular expressions that describe the amino acid sequence, allowing for some degree of variability. For example, a PROSITE pattern might specify a conserved sequence where certain positions can tolerate a limited range of amino acids.
* **``pfscan``:** A tool to scan protein sequences for PROSITE patterns. It uses predefined patterns (regular expressions) to scan sequences, looking for exact or near-exact matches to the specified patterns in the PROSITE database.
* **PROSITE Patterns vs Profiles:** PROSITE patterns are simple, descriptive motifs representing conserved sequences, while PROSITE profiles are detailed, position-specific scoring matrices that offer a more sensitive and comprehensive means of identifying and classifying protein domains and families. Both are used in the PROSITE database for annotating and predicting protein functions.

PROSITE Profiles
----------------

PROSITE is a database of protein domains, families, and functional sites. It contains biologically significant sites and patterns that help in identifying these features in protein sequences. PROSITE is widely used for protein annotation and to predict the function of newly discovered proteins based on their sequence similarity to known patterns.

1. ``PROSITE_RUNNER``: The protein sequences are analaysed by the PROSITE perl script ``ps_scan.pl``, using the PROSITE Profile models (``prosite_profiles.dat``) and evaluator models (``evaluator.dat``), by coordinating running ``pfsearchV3``.
2. ``PROSITE_PARSER``: The output from ``ps_scan.pl`` is parsed into the internal ``IPS6`` JSON structure by an in-house Python script, which filteres out all matches that do not have a match level of 'ONE', 'ZERO', 'MINUS_ONE'.

* **PROSITE Profile:** PROSITE profiles are more complex and sensitive than patterns. They are position-specific scoring matrices (PSSMs) that provide a quantitative measure of how well a sequence fits a particular protein domain or family. Profiles capture the variability at each position in the sequence, assigning scores based on the likelihood of observing each amino acid at each position. Profiles can detect more distant relationships than patterns, and are particularly useful for identifying members of protein families that have diverged significantly, i.e. where simple patterns might fail.
* **``pfsearch``:** A tool to search protein sequences against a database of PROSITE profiles. It uses profiles (position-specific scoring matrices) to perform searches, which allows for the detection of distant evolutionary relationships and more subtle sequence features. ``pfsearch`` compares the input protein sequences to the profiles in the PROSITE database and calculates scores to identify matches.
* **PROSITE Patterns vs Profiles:** PROSITE patterns are simple, descriptive motifs representing conserved sequences, while PROSITE profiles are detailed, position-specific scoring matrices that offer a more sensitive and comprehensive means of identifying and classifying protein domains and families. Both are used in the PROSITE database for annotating and predicting protein functions.

SignalP
-------

``SignalP`` is a bioinformatic tool for the prediction of the signal peptides and the location of their cleavage sites.

1. ``SIGNALP_RUNNER``: The protein sequences are parsed using the sequence analysis tool ``SignalP`` to predict the presence of signal peptides.
2. ``SIGNALP_PARSER``: The output from ``SignalP`` is parsed into the internal IPS6 JSON structure, using an in-house python script.

**Signal peptides:** A signal peptide, also known as a signal sequence, localisation sequence, or leader peptide, is a short peptide (protein sequence) that is usually 16-30 amino acids long. It is present at the N-terminus (or occasionally at the C-terminus or internally) of most newly synthesised proteins that are destined toward the secretory pathway. The role of the signal peptide is to prompt the transportation of the protein to a specific region of the cell, often the cell membrane. The signal peptide is typically cleaved following the succcessfully translocation of the protein.

(Deep)TMHMM
-----------

TMHMM is used to predict the presence of transmembrane domains within protein sequences. DeepTMHMM specifically uses deep leearning methos to predicte the membrane topology of transmembrane proteins. The model employed by DeepTMHMM encodes the primary amino acid sequence by a pre-trained language model and decodes the topology by a state space model to produce topology and type predictions at unprecedented accuracy.

1. ``TMHMM_RUNNER``: The protein sequences are parsed using the sequence analysis toole ``DeepTHMM``
2. ``TMHMM_PARSER``: The output from ``SignalP`` is parsed into the internal IPS6 JSON structure, using an in-house python script.

**Transmembrane regions:** The transmembrane region/domain in a protein sequence is the region of the protein that spans the entirety of the cell membrane. Transmembrane regions are typically composed of hydrophobic (water repelling) amino acids, forming a structure that is compatible with the hydrophobic environment between the lipid bilayers of the cell membrane.
