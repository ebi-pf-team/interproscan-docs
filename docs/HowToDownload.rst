Obtaining a copy of InterProScan 5
==================================

Firstly check your system satisfies the :ref:`Installation requirements`.
To install the InterProScan 5 software you then need to complete the following steps:

- Install the core InterProScan
- Install Panther Models
- Install/configure the Pre-calculated Match Lookup

Obtaining the core InterProScan software
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

64 bit distribution
^^^^^^^^^^^^^^^^^^^

::

    mkdir my_interproscan
    cd my_interproscan
    wget ftp://ftp.ebi.ac.uk/pub/software/unix/iprscan/5/5.44-79.0/interproscan-5.44-79.0-64-bit.tar.gz
    wget ftp://ftp.ebi.ac.uk/pub/software/unix/iprscan/5/5.44-79.0/interproscan-5.44-79.0-64-bit.tar.gz.md5

    # Recommended checksum to confirm the download was successful:
    md5sum -c interproscan-5.44-79.0-64-bit.tar.gz.md5
    # Must return *interproscan-5.44-79.0-64-bit.tar.gz: OK*
    # If not - try downloading the file again as it may be a corrupted copy.

(Direct link:
ftp://ftp.ebi.ac.uk/pub/software/unix/iprscan/5/5.44-79.0/interproscan-5.44-79.0-64-bit.tar.gz)

As the compressed file is very large, it is **strongly recommended**
that you use md5sum to check that the file has been downloaded without
errors, as described above.

Extract the tar ball:

::

    tar -pxvzf interproscan-5.44-79.0-*-bit.tar.gz

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

Installing Panther models
~~~~~~~~~~~~~~~~~~~~~~~~~

InterProScan 5 includes the `Panther <http://www.pantherdb.org/>`__
member database analysis.

Before installing Panther data
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

First ensure you have extracted the distribution of InterProScan 5

The path where this is extracted will be referred to below as
``[InterProScan5 home]``.

Download the Panther model data
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Download the latest Panther data files from the FTP site into the
``[InterProScan5 home]/data/`` directory:

::

    cd [InterProScan5 home]/data/

    wget ftp://ftp.ebi.ac.uk/pub/software/unix/iprscan/5/data/panther-data-14.1.tar.gz
    wget ftp://ftp.ebi.ac.uk/pub/software/unix/iprscan/5/data/panther-data-14.1.tar.gz.md5

This is a very large file so it is **strongly recommended** that you
check the MD5 checksum on the file after you have downloaded it:

::

    md5sum -c panther-data-14.1.tar.gz.md5
    # This must return *panther-data-14.1.tar.gz: OK*
    # If not - try downloading the file again as it may be a corrupted copy.

Install Panther data
^^^^^^^^^^^^^^^^^^^^

Extract the Panther data files to the required location, e.g.

::

    cd [InterProScan5 home]/data

    tar -pxvzf panther-data-14.1.tar.gz

OPTIONAL: Installing Panther in a different location
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

You may wish to install the Panther data files in a different location
on the file system. If you do this, you will need to edit the
``[InterProScan5 home]/interproscan.properties`` file and set the
following property to point to the correct location:

::

    panther.models.dir=PATH_TO/panther/14.1/
    panther.hmm.path=PATH_TO/panther/14.1/panther.hmm

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
