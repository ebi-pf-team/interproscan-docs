Running InterProScan 5 in CONVERT mode
======================================

InterProScan 5's CONVERT mode allows you to reformat an existing
InterProScan XML result file into any other possible output format (TSV,
GFF3, JSON). For compatibility reasons you can also convert XML
results into InterProScan 4.8 raw format (RAW). This will give our users
enough time to migrate their pipeline to InterProScan 5.

**Please note** it is NOT possible to reformat any non-XML format. XML
is the richest data type and is therefore the only format which allows
us to produce any other format of interest.

For more information on InterProScan formats available see `output
formats <https://github.com/ebi-pf-team/interproscan/wiki/OutputFormats>`__.

To enable InterProScan 5 to run in CONVERT mode you need to set the mode
option to 'CONVERT'.

Usage instructions
~~~~~~~~~~~~~~~~~~

::

    ./interproscan.sh -mode convert

You will see the following usage instructions:

::

    Welcome to InterProScan 5RC7
    usage: java -XX:+UseParallelGC -XX:+AggressiveOpts
                -XX:+UseFastAccessorMethods -Xms512M -Xmx2048M -jar
                interproscan-5.jar

    Please give us your feedback by sending an email to
    interhelp@ebi.ac.uk
     -b,--output-file-base <OUTPUT-FILE-BASE>   Optional, base output filename
                                                (relative or absolute path).
                                                Note that this option and the
                                                --outfile (-o) option are
                                                mutually exclusive.  The
                                                appropriate file extension for
                                                the output format(s) will be
                                                appended automatically. By
                                                default the input file
                                                path/name will be used.
     -d,--output-dir <OUTPUT-DIR>               Optional, output directory.
                                                Note that this option and the
                                                --outfile (-o) option or the
                                                --output-file-base (-b) option
                                                are mutually exclusive. The
                                                appropriate file extension for
                                                the output format(s) will be
                                                appended automatically. By
                                                default the input file
                                                path/name will be used.
     -f,--formats <OUTPUT-FORMATS>              Optional, case-insensitive,
                                                comma separated list of output
                                                formats. Supported formats are
                                                TSV, XML, JSON, and GFF3.
                                                Default for protein sequences
                                                are TSV, XML and GFF3, or
                                                for nucleotide sequences
                                                GFF3 and XML.
     -i,--input <INPUT-FILE-PATH>               Optional, path to fasta file
                                                that should be loaded on
                                                Master startup. Alternatively,
                                                in CONVERT mode, the
                                                InterProScan 5 XML file to
                                                convert.
     -o,--outfile <EXPLICIT_OUTPUT_FILENAME>    Optional explicit output file
                                                name (relative or absolute
                                                path).  Note that this option
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
     -T,--tempdir <TEMP-DIR>                    Optional, specify temporary
                                                file directory (relative or
                                                absolute path). The default
                                                location is temp/.
    Copyright (c) EMBL European Bioinformatics Institute, Hinxton, Cambridge,
    UK. (http://www.ebi.ac.uk) The InterProScan software itself is provided
    under the Apache License, Version 2.0
    (http://www.apache.org/licenses/LICENSE-2.0.html). Third party components
    (e.g. member database binaries and models) are subject to separate
    licensing - please see the individual member database websites for
    details.

Example Usage
~~~~~~~~~~~~~

::

    # Convert from XML format to all other available formats
    ./interproscan.sh -mode convert -f tsv,gff3,raw -i /path/to/existing_output_file.xml -b /path/to/output_file_basename

    # Convert from XML format to TSV format (which automatically includes all available InterPro entry/GO term/pathways information)
    ./interproscan.sh -i /path/to/existing_output_file.xml -mode convert -f tsv -o /path/to/new_output_file.tsv
