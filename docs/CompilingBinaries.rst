Installing and compiling binaries used in Interproscan
======================================================

The binaries that we distribute with InterProScan should work on most
linux systems. However, in some cases they may not work on a particular
system. If you are `trying to run InterProScan <HowToRun>`__ and you get
an error then you may need to compile the binary causing the error on
your own system in order for it to work.

Once a binary has been compiled you can either: - Replace the binary in
the relevant bin subdirectory in your InterProScan installation with
your newly compiled version - Or update the location of the binary in
your interproscan.properties configuration to point to your newly
compiled version

InterProScan is designed to work with the same binary versions as used
by the supported member database analyses. Therefore it is important to
use the binary version numbers listed below, see `the
FAQ <https://github.com/ebi-pf-team/interproscan/wiki/FAQ#5can-i-use-different-binary-versions-than-listed>`__
for more information.

cath-resolve-hits (used by CATH-Gene3D)
---------------------------------------

cath-resolve-hits is a tool written c/c++ and is used as part of the
postprocessing for CATH-Gene3D. The binary bundled in InterProScan
should work on most systems. If you get errors, download
cath-resolve-hits v0.15.2 that corresponds to your system from the
following page
https://github.com/UCLOrengoGroup/cath-tools/releases/tag/v0.15.2 into
bin/gene3d/4.2.0/ and rename it to bin/gene3d/4.2.0/cath-resolve-hits.

If the precompiled binary doesnt' solve your problems, compile the
binary for your system by following instructions on
http://cath-tools.readthedocs.io/en/latest/build/

Then either replace the relevant binary with your new one or update the
relevant interproscan.properties values to point at the new file
location. The default property values are:

::

    cath.resolve.hits.path=bin/gene3d/4.2.0/cath-resolve-hits

Pfscan/Pfsearch (used by ProSite Profiles, ProSite Patterns and HAMAP)
----------------------------------------------------------------------

Pfscan and pfsearch are written in fortran, so you may need to install
gfortran. On Ubuntu this can easily be done by:

::

    sudo apt-get install gfortran

Otherwise, source code and instructions for compiling gfortran can be
found at: http://gcc.gnu.org/wiki/GFortranBinaries

Next, download the source code and compile

::

    wget ftp://ftp.lausanne.isb-sib.ch/pub/software/unix/pftools/pft2.3/pft2.3.5.d.tar.gz
    tar -xzf pft2.3.5.d.tar.gz
    cd pftools/
    make io.o pfscan pfsearch

Then either replace the relevant files with your new ones or update the
relevant interproscan.properties values to point at the new file
locations. The default property values are:

::

    binary.prosite.pfscan.path=bin/prosite/pfscan
    binary.prosite.pfsearch.path=bin/prosite/pfsearch

Hmmer 2 (used by SMART)
-----------------------

::

    wget ftp://selab.janelia.org/pub/software/hmmer/2.3.2/hmmer-2.3.2.tar.gz
    tar -xzvf hmmer-2.3.2.tar.gz
    cd hmmer-2.3.2
    ./configure --enable-threads
    make
    make check
    make install

Then either replace the relevant binary with your new one or update the
relevant interproscan.properties values to point at the new file
location. The default property values are:

::

    binary.hmmer2.hmmsearch.path=bin/hmmer/hmmer2/2.3.2/hmmsearch
    binary.hmmer2.hmmpfam.path=bin/hmmer/hmmer2/2.3.2/hmmpfam

Hmmer 3 (used by CATH-Gene3D, HAMAP, PANTHER, Pfam, PIRSF, SFLD, SUPERFAMILY and TIGRFAMs)
------------------------------------------------------------------------------------------

Instructions for downloading and compiling Hmmer 3.1b1 can be found at:
http://hmmer.org/download.html

Then either replace the relevant binary with your new one or update the
relevant interproscan.properties values to point at the new file
location. The default property values are:

::

    binary.hmmer3.path=bin/hmmer/hmmer3/3.1b1
    binary.hmmer3.hmmscan.path=bin/hmmer/hmmer3/3.1b1/hmmscan
    binary.hmmer3.hmmsearch.path=bin/hmmer/hmmer3/3.1b1/hmmsearch

BLASTALL (used by ProDom)
-------------------------

**Version 2.2.19**

Download and unpack the suitable 32-bit OR 64-bit binary for your
system, for example:

::

    a)
    wget ftp://ftp.ncbi.nlm.nih.gov/blast/executables/legacy.NOTSUPPORTED/2.2.19/blast-2.2.19-ia32-linux.tar.gz
    b)
    wget ftp://ftp.ncbi.nlm.nih.gov/blast/executables/legacy.NOTSUPPORTED/2.2.19/blast-2.2.19-ia64-linux.tar.gz
    tar -xzf blast-2.2.19-ia<bit-version>-linux.tar.gz

Then either replace the relevant binary with your new one or update the
relevant interproscan.properties values to point at the new file
location. The default property values are:

::

    binary.blast.2.2.19.path=bin/blast/2.2.19

**Note:** If no suitable binary is available for your system at
ftp://ftp.ncbi.nlm.nih.gov/blast/executables/legacy.NOTSUPPORTED/2.2.19/
then you can try to compile the source code yourself on your system. The
source can be found in the corresponding version of the NCBI toolkit at
ftp://ftp.ncbi.nlm.nih.gov/toolbox/ncbi_tools/old/20081116/

EMBOSS getorf (for nucleic acid sequence search)
------------------------------------------------

InterProScan 5 takes advantage of the Open Reading Frame (ORF)
prediction tool Emboss getorf.

How to install on Debian based systems?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

On Debian based systems (e.g. Ubuntu or Linux Mint) EMBOSS is available
as a package (see http://packages.debian.org/sid/emboss) produced by
Debain Med (http://www.debian.org/devel/debian-med/) and can be
installed with:

::

    $ sudo apt-get install emboss

Issues with unnecessary dependencies
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

If you have problems with unnecessary dependencies and you get error
messages like the following...

::

    bin/nucleotide/getorf: error while loading shared libraries: ...

... then try getting a binary with less dependencies from
ftp://ftp.ebi.ac.uk/pub/databases/interpro/iprscan/5/bin/getorf.zip.
Unzip getorf.zip into bin/nucleotide/

... or try compiling EMBOSS with appropriate 'configure' options
**might** solve that (but not necessarily).

For example:

::

    ./configure --disable-shared --without-x --without-java --without-hpdf --without-pngdriver --without-mysql --without-postgresql

Get EMBOSS sources from EMBOSS [ftp://emboss.open-bio.org/pub/EMBOSS/]

Note: additional options may be required with other versions of EMBOSS,
this example uses EMBOSS 6.3.1.

To disable, and thus remove dependencies on, MySQL, Postgres, Axis 2,
X11, PDF support (libhpdf) and PNG support (libpng), and thus reduce the
dependencies from:

::

    $ ldd interproscan-5-RC7/bin/nucleotide/getorf
        linux-vdso.so.1 =>  (0x00007fff033ff000)
        libmysqlclient.so.16 => /usr/lib64/mysql/libmysqlclient.so.16 (0x0000003bf0600000)
        libcrypt.so.1 => /lib64/libcrypt.so.1 (0x0000003492000000)
        libnsl.so.1 => /lib64/libnsl.so.1 (0x00000034e7a00000)
        libssl.so.10 => /usr/lib64/libssl.so.10 (0x00000034df600000)
        libcrypto.so.10 => /usr/lib64/libcrypto.so.10 (0x00000034dd200000)
        libpq.so.5 => /usr/lib64/libpq.so.5 (0x0000003bf0e00000)
        libX11.so.6 => /usr/lib64/libX11.so.6 (0x00000034db200000)
        libgd.so.2 => /usr/lib64/libgd.so.2 (0x0000003bf0a00000)
        libpng12.so.0 => /usr/lib64/libpng12.so.0 (0x00000034dee00000)
        libz.so.1 => /lib64/libz.so.1 (0x00000034d9200000)
        libm.so.6 => /lib64/libm.so.6 (0x00000034d8200000)
        libc.so.6 => /lib64/libc.so.6 (0x00000034d8600000)
        libfreebl3.so => /lib64/libfreebl3.so (0x0000003491800000)
        libgssapi_krb5.so.2 => /lib64/libgssapi_krb5.so.2 (0x00000034de600000)
        libkrb5.so.3 => /lib64/libkrb5.so.3 (0x00000034de200000)
        libcom_err.so.2 => /lib64/libcom_err.so.2 (0x00000034dc600000)
        libk5crypto.so.3 => /lib64/libk5crypto.so.3 (0x00000034dca00000)
        libdl.so.2 => /lib64/libdl.so.2 (0x00000034d8a00000)
        libldap_r-2.4.so.2 => /lib64/libldap_r-2.4.so.2 (0x00007f8b99cbf000)
        libpthread.so.0 => /lib64/libpthread.so.0 (0x00000034d8e00000)
        libxcb.so.1 => /usr/lib64/libxcb.so.1 (0x00000034dba00000)
        libXpm.so.4 => /usr/lib64/libXpm.so.4 (0x000000348dc00000)
        libjpeg.so.62 => /usr/lib64/libjpeg.so.62 (0x00000034eb000000)
        libfontconfig.so.1 => /usr/lib64/libfontconfig.so.1 (0x00000034dfa00000)
        libfreetype.so.6 => /usr/lib64/libfreetype.so.6 (0x00000034df200000)
        /lib64/ld-linux-x86-64.so.2 (0x00000034d7e00000)
        libkrb5support.so.0 => /lib64/libkrb5support.so.0 (0x00000034dde00000)
        libkeyutils.so.1 => /lib64/libkeyutils.so.1 (0x00000034dc200000)
        libresolv.so.2 => /lib64/libresolv.so.2 (0x00000034da200000)
        liblber-2.4.so.2 => /lib64/liblber-2.4.so.2 (0x00000034eb800000)
        libssl3.so => /usr/lib64/libssl3.so (0x00000034eac00000)
        libsmime3.so => /usr/lib64/libsmime3.so (0x00000034ea800000)
        libnss3.so => /usr/lib64/libnss3.so (0x00000034ea400000)
        libnssutil3.so => /usr/lib64/libnssutil3.so (0x00000034e9c00000)
        libplds4.so => /lib64/libplds4.so (0x00000034ea000000)
        libplc4.so => /lib64/libplc4.so (0x00000034e9400000)
        libnspr4.so => /lib64/libnspr4.so (0x00000034e9800000)
        libsasl2.so.2 => /usr/lib64/libsasl2.so.2 (0x0000003490000000)
        libXau.so.6 => /usr/lib64/libXau.so.6 (0x00000034db600000)
        libexpat.so.1 => /lib64/libexpat.so.1 (0x00000034dea00000)
        libselinux.so.1 => /lib64/libselinux.so.1 (0x00000034d9a00000)

to a more portable:

::

    $ ldd emboss/getorf
        linux-vdso.so.1 =>  (0x00007fff88bff000)
        libm.so.6 => /lib64/libm.so.6 (0x00000034d8200000)
        libc.so.6 => /lib64/libc.so.6 (0x00000034d8600000)
        /lib64/ld-linux-x86-64.so.2 (0x00000034d7e00000)

You will also find a discussion thread about that here [[Issue 11 \|
https://code.google.com/p/interproscan/issues/detail?id=11]] (on Google
Code).

Then either replace the relevant binary with your new one or update the
relevant interproscan.properties values to point at the new file
location. The default property values are:

::

    binary.getorf.path=bin/nucleotide/getorf

ncoils (used by Coils)
----------------------

Instructions for compiling the "ncoils" binary can be found in the
src/coils/ncoils/2.2.1/README file in your `extracted InterProScan 5
distribution <HowToDownload>`__ (release 5.17-56.0 onwards).

Then either replace the relevant binary with your new one or update the
relevant interproscan.properties values to point at the new file
location. The default property values are:

::

    binary.coils.path=bin/ncoils/2.2.1/ncoils

fingerPRINTScan (used by PRINTS)
--------------------------------

Instructions for compiling the "fingerPRINTScan" binary can be found in
the src/prints/fingerprintscan/3597/INSTALL file in your extracted
InterProScan 5 distribution (release 5.17-56.0 onwards) and are
summarised as below:

::

    cd src/prints/fingerprintscan/3597/
    ./configure
    make
    cd _interproscan_dir
    cp src/prints/fingerprintscan/3597/fingerPRINTScan bin/prints/

where "\_interproscan\_dir" is the directory where you have installed
InterProScan 5.

If you choose not to replace the relevant binary with your new one then
instead you can update the relevant interproscan.properties values to
point at the new file location. The default property values are:

::

    binary.fingerprintscan.path=bin/prints/fingerPRINTScan

rpsblast/rpsbproc (used by CDD)
-------------------------------

Instructions on how to compile rpsblast/rpsbproc for interproscan are
summarised as follows:

First check the c++ compiler version

::

    c++ --version

if the c++ version is less than 4.8 compilation will most likely fail
and you should upgrade to a c++ compiler version 4.8 or above.

If you have a c++ version 4.8 or above then follow the instructions
below.

::

    mkdir cddblast
    cd cddblast
    wget ftp://ftp.ncbi.nih.gov/blast/executables/blast+/2.6.0/ncbi-blast-2.6.0+-src.tar.gz
    wget ftp://ftp.ncbi.nih.gov/blast/executables/blast+/2.6.0/ncbi-blast-2.6.0+-src.tar.gz.md5
    md5sum -c ncbi-blast-2.6.0+-src.tar.gz.md5
    # Above command should return "ncbi-blast-2.6.0+-src.tar.gz: OK" if download successful
    tar xvzf ncbi-blast-2.6.0+-src.tar.gz
    cd ncbi-blast-2.6.0+-src/c++/src/app/
    wget -r --no-parent -l 1 -np -nd -nH -P rpsbproc ftp://ftp.ncbi.nih.gov/pub/mmdb/cdd/rpsbproc/rpsbproc-src/
    #edit Makefile.in and make sure SUB_PROJ is assigned two applications as follows: SUB_PROJ = blast rpsbproc
    cd ../../
    ./configure
    /usr/bin/make
    #after compilation is complete
    cp ReleaseMT/bin/rpsblast <interproscan_install_dir>/bin/blast/ncbi-blast-2.6.0+/
    cp ReleaseMT/bin/rpsbproc <interproscan_install_dir>/bin/blast/ncbi-blast-2.6.0+/

The complete instruction set can be found here:
ftp://ftp.ncbi.nih.gov/pub/mmdb/cdd/rpsbproc/README

If you choose not to replace the relevant binary with your new one then
instead you can update the relevant interproscan.properties values to
point at the new file location. The default property values are:

::

    binary.rpsblast.path=bin/blast/ncbi-blast-2.6.0+/rpsblast
    binary.rpsbproc.path=bin/blast/ncbi-blast-2.6.0+/rpsbproc

sfld\_preprocess/sfld\_postprocess (used by SFLD)
-------------------------------------------------

Instructions for compiling the "sfld\_preprocess" and
"sfld\_postprocess" binaries can be found in the src/sfld/1/README file
in your `extracted InterProScan 5 distribution <HowToDownload>`__
(release 5.22-61.0 onwards).

Then either replace the relevant binary with your new one or update the
relevant interproscan.properties values to point at the new file
location. The default property values are:

::

    sfld.postprocess.command=bin/sfld/sfld_postprocess

Phobius, TMHMM or SignalP
-------------------------

By default the Phobius, SignalP and TMHMM member database analyses are
deactivated because they contain licensed components. For instructions
on how to activate these analyses, obtain the relevant licenses and
compile the binaries please see "`activating licensed
analyses <ActivatingLicensedAnalyses>`__".
