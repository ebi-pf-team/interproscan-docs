Obtaining a copy of InterProScan 5
==================================

Firstly check your system satisfies the :ref:`Installation requirements`.
To install the InterProScan 5 software you then need to complete the following steps:

- Install the core InterProScan
- Install/configure the Pre-calculated Match Lookup

Obtaining the core InterProScan software
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

::

    mkdir my_interproscan
    cd my_interproscan
    wget ftp://ftp.ebi.ac.uk/pub/software/unix/iprscan/5/5.47-82.0/interproscan-5.47-82.0-64-bit.tar.gz
    wget ftp://ftp.ebi.ac.uk/pub/software/unix/iprscan/5/5.47-82.0/interproscan-5.47-82.0-64-bit.tar.gz.md5

    # Recommended checksum to confirm the download was successful:
    md5sum -c interproscan-5.47-82.0-64-bit.tar.gz.md5
    # Must return *interproscan-5.47-82.0-64-bit.tar.gz: OK*
    # If not - try downloading the file again as it may be a corrupted copy.

(Direct link:
ftp://ftp.ebi.ac.uk/pub/software/unix/iprscan/5/5.47-82.0/interproscan-5.47-82.0-64-bit.tar.gz)

As the compressed file is very large, it is **strongly recommended**
that you use md5sum to check that the file has been downloaded without
errors, as described above.

Extract the tar ball:

::

    tar -pxvzf interproscan-5.47-82.0-*-bit.tar.gz

    # where:
    #     p = preserve the file permissions
    #     x = extract files from an archive
    #     v = verbosely list the files processed
    #     z = filter the archive through gzip
    #     f = use archive file

This is a completely self-contained version that includes member
database specific binaries and model / signature files. This should run
'out of the box' on a Linux system. Note that it excludes Panther (which
can be downloaded separately - see below) and analyses that contain
components for which you are obliged to acquire your own license.

Panther models
~~~~~~~~~~~~~~~~~~~~~~~~~
Previous versions of InterProScan required a separate installation of Panther data. Starting with interproscan-5.47-82.0
onwards, this is not necessary. Panther data is bundled together with the rest of the application data.

Using the Local Pre-calculated Match Lookup Service (optional)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The pre-calculated match lookup web service is able to provide matches
to more than 30 million protein sequences, including all of the sequence
in UniProtKB. By default InterProScan 5 is configured (in the
interproscan.properties file) to use the web service hosted at the EBI.
Your servers will need to have external access to http://www.ebi.ac.uk
to use it.

InterProScan 5 uses this service to retrieve pre-calculated matches,
reducing the need for compute on your server and speeding up the
response time.

If you are behind a firewall that prevents such access and you are
unable to configure access, you could **either**
:ref:`What is the InterProScan 5 Lookup Service?`
**or** turn off the use of this service, which means the
analysis will run locally without any match lookup

To turn off the use of the service, either use the -dp command line
option or edit **interproscan.properties** and add a # to the start of
the following line to comment out the line or delete the following line,
near the bottom of the file:

::

    precalculated.match.lookup.service.url=http://www.ebi.ac.uk/interpro/match-lookup

It is important to note that we run the latest available version of the
pre-calculated match lookup service at the EBI. In the event of a new
release, you will be required to either install the latest version of
InterProScan 5, or to `install the required version of the lookup
service locally <LocalLookupService>`__.
