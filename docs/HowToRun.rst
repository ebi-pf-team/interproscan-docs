Running InterProScan 5
======================

Once you have uncompressed your :ref:`Obtaining a copy of InterProScan 5`, you can
run InterProScan directly from the command line.

Run the supplied shell script. If you run this script with no arguments,
you will be presented with the usage instructions:

::

    ./interproscan.sh

After a short delay, you will see the following **usage instructions**:

::

    Welcome to InterProScan-5.XX-XX.X
    usage: java -XX:+UseParallelGC -XX:+AggressiveOpts
                -XX:+UseFastAccessorMethods -Xms128M -Xmx2048M -jar
                interproscan-5.jar

    Please give us your feedback by sending an email to

    interhelp@ebi.ac.uk

     -appl,--applications <ANALYSES>            Optional, comma separated list
                                                of analyses.  If this option
                                                is not set, ALL analyses will
                                                be run.
     -b,--output-file-base <OUTPUT-FILE-BASE>   Optional, base output filename
                                                (relative or absolute path).
                                                Note that this option, the
                                                --output-dir (-d) option and
                                                the --outfile (-o) option are
                                                mutually exclusive.  The
                                                appropriate file extension for
                                                the output format(s) will be
                                                appended automatically. By
                                                default the input file
                                                path/name will be used.
     -d,--output-dir <OUTPUT-DIR>               Optional, output directory.
                                                Note that this option, the
                                                --outfile (-o) option and the
                                                --output-file-base (-b) option
                                                are mutually exclusive. The
                                                output filename(s) are the
                                                same as the input filename,
                                                with the appropriate file
                                                extension(s) for the output
                                                format(s) appended
                                                automatically .
     -dp,--disable-precalc                      Optional.  Disables use of the
                                                precalculated match lookup
                                                service.  All match
                                                calculations will be run
                                                locally.
     -dra,--disable-residue-annot               Optional, excludes sites from
                                                the XML, JSON output
     -f,--formats <OUTPUT-FORMATS>              Optional, case-insensitive,
                                                comma separated list of output
                                                formats. Supported formats are
                                                TSV, XML, JSON, GFF3, HTML and
                                                SVG. Default for protein
                                                sequences are TSV, XML and
                                                GFF3, or for nucleotide
                                                sequences GFF3 and XML.
     -goterms,--goterms                         Optional, switch on lookup of
                                                corresponding Gene Ontology
                                                annotation (IMPLIES -iprlookup
                                                option)
     -help,--help                               Optional, display help
                                                information
     -i,--input <INPUT-FILE-PATH>               Optional, path to fasta file
                                                that should be loaded on
                                                Master startup. Alternatively,
                                                in CONVERT mode, the
                                                InterProScan 5 XML file to
                                                convert.
     -iprlookup,--iprlookup                     Also include lookup of
                                                corresponding InterPro
                                                annotation in the TSV and GFF3
                                                output formats.
     -ms,--minsize <MINIMUM-SIZE>               Optional, minimum nucleotide
                                                size of ORF to report. Will
                                                only be considered if n is
                                                specified as a sequence type.
                                                Please be aware of the fact
                                                that if you specify a too
                                                short value it might be that
                                                the analysis takes a very long
                                                time!
     -o,--outfile <EXPLICIT_OUTPUT_FILENAME>    Optional explicit output file
                                                name (relative or absolute
                                                path).  Note that this option,
                                                the --output-dir (-d) option
                                                and the --output-file-base
                                                (-b) option are mutually
                                                exclusive. If this option is
                                                given, you MUST specify a
                                                single output format using the
                                                -f option.  The output file
                                                name will not be modified.
                                                Note that specifying an output
                                                file name using this option
                                                OVERWRITES ANY EXISTING FILE.
     -pa,--pathways                             Optional, switch on lookup of
                                                corresponding Pathway
                                                annotation (IMPLIES -iprlookup
                                                option)
     -t,--seqtype <SEQUENCE-TYPE>               Optional, the type of the
                                                input sequences (dna/rna (n)
                                                or protein (p)).  The default
                                                sequence type is protein.
     -T,--tempdir <TEMP-DIR>                    Optional, specify temporary
                                                file directory (relative or
                                                absolute path). The default
                                                location is temp/.
     -version,--version                         Optional, display version
                                                number

    Copyright (c) EMBL European Bioinformatics Institute, Hinxton, Cambridge,
    UK. (http://www.ebi.ac.uk) The InterProScan software itself is provided
    under the Apache License, Version 2.0
    (http://www.apache.org/licenses/LICENSE-2.0.html). Third party components
    (e.g. member database binaries and models) are subject to separate
    licensing - please see the individual member database websites for
    details.
    Available analyses:
                          TIGRFAM (XX.X) : TIGRFAMs are protein families based on Hidden Markov Models or HMMs
                             SFLD (X.X) : SFLDs are protein families based on Hidden Markov Models or HMMs
                            ProDom (XXXX.X) : ProDom is a comprehensive set of protein domain families automatically generated from the UniProt Knowledge Database.
                            Hamap (XXXXXX.XX) : High-quality Automated and Manual Annotation of Microbial Proteomes
                            SMART (X.X) : SMART allows the identification and analysis of domain architectures based on Hidden Markov Models or HMMs
                              CDD (X.XX) : Prediction of CDD domains in Proteins
                  ProSiteProfiles (XX.XXX) : PROSITE consists of documentation entries describing protein domains, families and functional sites as well as associated patterns and profiles to identify them
                  ProSitePatterns (XX.XXX) : PROSITE consists of documentation entries describing protein domains, families and functional sites as well as associated patterns and profiles to identify them
                      SUPERFAMILY (X.XX) : SUPERFAMILY is a database of structural and functional annotation for all proteins and genomes.
                           PRINTS (XX.X) : A fingerprint is a group of conserved motifs used to characterise a protein family
                          PANTHER (X.X) : The PANTHER (Protein ANalysis THrough Evolutionary Relationships) Classification System is a unique resource that classifies genes by their functions, using published scientific experimental evidence and evolutionary relationships to predict function even in the absence of direct experimental evidence.
                           Gene3D (X.X.X) : Structural assignment for whole genes and genomes using the CATH domain structure database
                            PIRSF (X.XX) : The PIRSF concept is being used as a guiding principle to provide comprehensive and non-overlapping clustering of UniProtKB sequences into a hierarchical order to reflect their evolutionary relationships.
                             Pfam (XX.X) : A large collection of protein families, each represented by multiple sequence alignments and hidden Markov models (HMMs)
                            Coils (X.X) : Prediction of Coiled Coil Regions in Proteins
                       MobiDBLite (X.X) : Prediction of disordered domains Regions in Proteins

    Deactivated analyses:
            SignalP_GRAM_POSITIVE (X.X) : Analysis SignalP_GRAM_POSITIVE-X.X is deactivated, because the following parameters are not set in the interproscan.properties file: binary.signalp.4.0.path
                      SignalP_EUK (X.X) : Analysis SignalP_EUK-X.X is deactivated, because the following parameters are not set in the interproscan.properties file: binary.signalp.4.0.path
                          Phobius (X.XX) : Analysis Phobius-X.XX is deactivated, because the following parameters are not set in the interproscan.properties file: binary.phobius.pl.path.1.01
                            TMHMM (X.Xc) : Analysis TMHMM-X.Xc is deactivated, because the following parameters are not set in the interproscan.properties file: binary.tmhmm.path, tmhmm.model.path
            SignalP_GRAM_NEGATIVE (X.X) : Analysis SignalP_GRAM_NEGATIVE-X.X is deactivated, because the following parameters are not set in the interproscan.properties file: binary.signalp.4.0.path

The latest analysis versions can be obtained by running the InterProScan
script without any options specified.

InterProScan 5 test run
~~~~~~~~~~~~~~~~~~~~~~~

This distribution of InterProScan 5 provides a set of protein test
sequences, which you can use to check how InterProScan 5 behaves on your
system. First, if you have not yet run the initilisation script run the following command:
::
    python3 initial_setup.py

This command will  press and index the hmm models to prepare them into a format used by hmmscan. This command need only be run once.

You can then run the following two test case commands:
::
    ./interproscan.sh -i test_proteins.fasta -f tsv -dp
    ./interproscan.sh -i test_proteins.fasta -f tsv

The first test should create an output file with the default file name
test\_proteins.fasta.tsv, and the second would then create
test\_proteins.fasta\_1.tsv (since the default filename already exists).

**What should you get?**

InterProScan 5 should run through properly without any warnings and it
will create a TSV output file containing several member database
matches, including Gene3d, PIRSF etc.

The member database binaries supplied with InterProScan 5 should run on
most Linux systems, however if they don't work on a particular system
then see the FAQ page,
:ref:`What should I do if one of the binaries included with InterProScan 5 doesn't work on my system?`.

Command-line options
~~~~~~~~~~~~~~~~~~~~

-dp / --disable-precalc (optional)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

InterProScan is a computationally expensive program, sometimes taking a
couple of minutes to characterise a single sequence. It calculates
matches to InterPro signatures based purely on the amino acid sequence
that is submitted to it. Therefore, 2 identical amino acid sequences
will produce identical outputs (although if the sequences differ by just
one residue, the outputs may or may not be the same). We can take
advantage of this feature, and increase the speed of InterProScan, by
pre-calculating matches for sequences already found in UniProtKB. When a
sequence is submitted to it, InterProScan calculates an MD5 checksum for
the amino acid sequence and then uses that checksum to check the
:ref:`What is the InterProScan 5 Lookup Service?` 
`pre-calculated lookup service <PrecalculatedMatchLookup>`__ to see
whether it has already been encountered. If it has, the pre-calculated
results are returned to the user; if not, the InterProScan search
algorithms are run against the sequence.

By default, InterProScan has this option turned on. If you wish to turn
it off, you should add the "--disable-precalc" option to the command
line. Users also have the option of using an EBI-hosted instance of the
look-up service (this is what is enabled by default) or downloading a
copy and running it locally. For more information, read the section on
`configuring the match lookup
service <#Configuring_the_Pre-calculated_Match_Lookup_Service>`__ below

-appl / --applications *application\_name* (optional)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

By default, **all** available analyses are run, however if you wish to
restrict to a single analysis, use the **-appl** option. The argument to
the **-appl** option should be one of the analyses named at the bottom
of the usage instructions. Analysis names may or may not contain version
numbers. For example:

::

    ./interproscan.sh -appl Pfam -i /path/to/sequences.fasta

If you wish to specifically run two or more analyses you can include
multiple **-appl** arguments:

::

    ./interproscan.sh -appl Pfam-31.0 -appl PRINTS-42.0 -i /path/to/sequences.fasta

or you can use a single **-appl** option with a comma-separated list of
analyses:

::

    ./interproscan.sh -appl CDD,COILS,Gene3D,HAMAP,MobiDBLite,PANTHER,Pfam,PIRSF,PRINTS,ProDom,PROSITEPATTERNS,PROSITEPROFILES,SFLD,SMART,SUPERFAMILY,TIGRFAM -i /path/to/sequences.fasta

A list of all available analyses is in the section "`Included
Analyses <#included-analyses>`__"

-i / --fasta *sequence\_file*
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

To analyse the contents of a fasta file, you should add one argument as
in the following example:

::

    ./interproscan.sh -i /path/to/sequences.fasta

This will return results in the default formats as described above,
i.e., for protein sequences, return TSV, XML and GFF3 files or for
nucleotide sequences, return GFF3 and XML files with file names based
upon the name of the fasta file. (**sequences.tsv, sequence.xml,
sequences.gff3** in this case).

-iprlookup,--iprlookup (optional)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Option that provides mappings from matched member database signatures to
the InterPro entries that they are integrated into.

-goterms,--goterms (optional)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Option that provides mappings to the Gene Ontology (GO). These mappings
are based on the matched manually curated InterPro entries. (IMPLIES
-iprlookup option)

-b / --output-file-base *file\_name* (optional)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Optionally, you can supply a path and base name (excluding a file
extension) for the results file as follows:

::

    ./interproscan.sh -i /path/to/sequences.fasta -b /path/to/output_file

**The appropriate file extension will be added to each output file**,
depending upon the format(s) requested. (It is therefore recommended
that you do **not** include a file extension yourself.)

Note that using this option will **not** overwrite existing files. If a
file with the required name exists at the path specified, the provided
file name will have 'underscore\_number' appended in front of the file
extension.

-o / --outfile (optional)
^^^^^^^^^^^^^^^^^^^^^^^^^

This command can be given **instead** of the -b option. If you provide
this argument, you **must** specify a single output format. The output
file will be given the name specified by this option.

Note that this option **will overwrite** existing files with the same
path / name.

-pa / --pathways (optional)
^^^^^^^^^^^^^^^^^^^^^^^^^^^

Option that provides mappings from matches to pathway information, which
is based on the matched manually curated InterPro entries. (IMPLIES
-iprlookup option). The different pathways databases that I5 provides
cross links to are: \* KEGG \* MetaCyc \* Reactome

-t / --seqtype (optional)
^^^^^^^^^^^^^^^^^^^^^^^^^

InterProScan 5 supports analysis of both protein and nucleic acid
sequences (DNA/RNA). Your input sequences are interpreted as protein
sequences by default. If you like to scan nucleotide sequences you must
set the -t option:

::

    ./interproscan.sh -t n -i /path/to/sequences.fasta

-T / --tempdir (optional)
^^^^^^^^^^^^^^^^^^^^^^^^^

Optionally, you can specify the location of the InterProScan 5 temporary
directory. This directory is used as a working directory. The default
temporary directory will be in the same directory as the InterProScan 5
script file (interproscan.sh). By default, this directory is completely
cleaned up after InterProScan 5 finished all analyses successfully.

Example usage:

::

    ./interproscan.sh -T /path/to/temp-directory -i /path/to/sequences.fasta

-dra / --disable-residue-annot (optional)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Optionally, you can prevent InterProScan from calculating the residue
level annotations and displaying in the output where available. If you
don't require this information then disabling the feature will improve
performance and result in smaller output files.

-version / --version (optional)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Display the version number of the InterProScan software you are running.

Included analyses
~~~~~~~~~~~~~~~~~

This distribution of InterProScan includes:

- `CDD <http://www.ncbi.nlm.nih.gov/Structure/cdd/cdd.shtml>`__
- `COILS <http://www.ch.embnet.org/software/COILS_form.html>`__
- `Gene3D <http://gene3d.biochem.ucl.ac.uk/Gene3D/>`__
- `HAMAP <http://hamap.expasy.org/>`__
- `MOBIDB <http://mobidb.bio.unipd.it/>`__
- `PANTHER <http://www.pantherdb.org/>`__
- `Pfam <http://pfam.sanger.ac.uk/>`__
- `PIRSF <http://pir.georgetown.edu/pirwww/dbinfo/pirsf.shtml>`__
- `PRINTS <http://www.bioinf.manchester.ac.uk/dbbrowser/PRINTS/index.php>`__
- `ProDom <http://prodom.prabi.fr/prodom/current/html/home.php>`__
- `PROSITE <http://prosite.expasy.org/>`__ (Profiles and Patterns)
- `SFLD <http://sfld.rbvi.ucsf.edu/django/>`__
- `SMART <http://smart.embl-heidelberg.de/>`__ (unlicensed components only
  by default - this analysis has simplified post-processing that includes
  an E-value filter, however you should not expect it to give the same
  match output as the fully licensed version of SMART)
- `SUPERFAMILY <http://supfam.cs.bris.ac.uk/SUPERFAMILY/>`__
- `TIGRFAMs <http://www.jcvi.org/cgi-bin/tigrfams/index.cgi>`__

A number of other analyses are available in InterProScan 5. These
analyses use licensed code and data provided by third parties. If you
wish to run these analyses it will be necessary for you to obtain a
licence from the vendor and configure your local InterProScan 5
installation to use these:

- `Phobius <http://phobius.sbc.su.se/>`__ (licensed software)
- `SignalP <http://www.cbs.dtu.dk/services/SignalP/>`__
- `SMART <http://smart.embl-heidelberg.de/>`__ (licensed components)
- `TMHMM <http://www.cbs.dtu.dk/services/TMHMM/>`__

The InterPro team would like to thank the developers and maintainers of
all of these analyses for their valued and on-going support.

Output format
~~~~~~~~~~~~~

Please see :ref:`Output formats`.

Optional configuration
~~~~~~~~~~~~~~~~~~~~~~

Working directory for temporary files
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

There is a second way of changing temporary/working directory beyond the
-T option (where fasta files, binary output etc. are written to). You
can do this by editing the **interproscan.properties** file and change
the path for the property:

::

    temporary.file.directory=temp/[UNIQUE]

**NOTE**: Leave **/[!UNIQUE]** on the end - this is replaced with a
timestamped / unique directory for each run. This directory is cleaned
up and deleted at the end of each run of InterProScan 5.

Configuring the Pre-calculated Match Lookup Service
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

As this is a web service, your servers will need to have external access
to http://www.ebi.ac.uk to use it. If you are behind a firewall that
prevents such access and you are unable to configure access, you can
either turn off use of this service or download a copy and run a `local
match lookup service <LocalLookupService>`__.

To turn off use of the service, either use the -dp command line option,
or edit **interproscan.properties** and comment out\ ``*`` or delete the
following line, near the bottom of the file:

::

    precalculated.match.lookup.service.url=http://www.ebi.ac.uk/interpro/match-lookup

**``*``\ (To comment the line out, add a # to the start of the line.)**

Running InterProScan 5 on an LSF/SGE Cluster
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Please see :ref:`Cluster Mode`.
