Installing and compiling binaries used in Interproscan
======================================================

The binaries that we distribute with InterProScan should work on most
linux systems. However, in some cases they may not work on a particular
system. If you are `trying to run InterProScan <HowToRun.html>`__ and you get
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
FAQ <FAQ.html#5can-i-use-different-binary-versions-than-listed>`__
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

ncoils (used by Coils)
----------------------
If you get **Coils**  (ncoils) errors, you may need to compile the **Coils** binary and it is straight forward as follows.
::

  cd src/coils/ncoils/2.2.1
  make
  cd ../../../..
  cp src/coils/ncoils/2.2.1/ncoils bin/ncoils/2.2.1/ncoils

The steps above normally solve the problem.

Instructions for compiling the "ncoils" binary can also be found in the
src/coils/ncoils/2.2.1/README file in your `extracted InterProScan 5
distribution <HowToDownload.html>`__ (release 5.17-56.0 onwards).

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
There are two seperate application from NCBI that CDD uses for analysis in
InterProScan. If the applications rpsblast and rpsbproc provided in
InterProScan are not working for you,

- download rpsblast/rpsbproc from NCBI (`https://blast.ncbi.nlm.nih.gov/Blast.cgi <https://blast.ncbi.nlm.nih.gov/Blast.cgi>`__)

  - for rpsblast, it is part of the main blast package, so  download https://ftp.ncbi.nlm.nih.gov/blast/executables/blast+/LATEST/ncbi-blast-2.11.0+-x64-linux.tar.gz  and look for rpsblast after uncompressing the tar file.
  - for rpsbproc, get it from `ftp://ftp.ncbi.nih.gov/pub/mmdb/cdd/rpsbproc/ <ftp://ftp.ncbi.nih.gov/pub/mmdb/cdd/rpsbproc/>`__

- if they dont work, then you have to compile these binaries for your system.
We are working on a summary of how to compile rpsblast/rpsbproc for the latest
Blast release - ncbi-blast-2.11.0+.

For an older release ncbi-blast-2.6.0+, below are the  instructions. They could
be adapted to work for ncbi-blast-2.11.0+.

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
in your `extracted InterProScan 5 distribution <HowToDownload.html>`__
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
analyses <ActivatingLicensedAnalyses.html>`__".
