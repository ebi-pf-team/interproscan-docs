Known issues
============

Open issues in InterProScan
---------------------------

This page documents the latest list of known issues, and we are working to fix them
as soon as possible. For assistance with other InterProScan problems,
`please contact us using EMBL EBI's support form <http://www.ebi.ac.uk/support/interproscan>`__.


1. CDD/RPSBlast errors
~~~~~~~~~~~~~~~~~~~~~~~

On some linux systems, you may get rpsblast errors like
::
  bin/blast/ncbi-blast-2.10.1+/rpsbproc: error while loading shared libraries: libgomp.so.1: cannot open shared object file: No such file or directory

The missing library is libgomp1. On Ubuntu you might install it as follows:
::
  sudo apt-get install -y libgomp1

On other systems, you have similar installation commands

2. Coils errors
~~~~~~~~~~~~~~~~
If you see an error concerning **Coils**, for example the error below, it means the binary
we provide is not compatible with your system.
::

  Cannot run program ".../bin/ncoils/2.2.1/ncoils": error=2, No such file or directory


In this case, you may need to compile the **Coils** binary and it is straight forward as follows.
::
  cd src/coils/ncoils/2.2.1
  make
  cd ../../../..
  cp src/coils/ncoils/2.2.1/ncoils bin/ncoils/2.2.1/ncoils

These steps should update the Coils binary.

3. Prosite/pfsearchV3 errors
~~~~~~~~~~~~~~~~

On some linux systems, you may get pfsearchV3 errors like

::
  Error output from binary: bin/prosite/pfsearchV3: error while loading shared libraries: libpcre2.so: cannot open shared object file: No such file or directory

The missing library is pcre2. On Ubuntu you might install it as follows:
::
  sudo apt-get install -y libpcre2-dev

On other systems, you have similar installation commands.


Additionally on some systems, specially on cluster environments, the following error has been reported
::
  Error setting affinity!
  Error running prosite binary bin/prosite/pfsearchV3

This means the provided binary is not compatible with your system. We have included alternative binaries
that you can try, by updating you **interproscan.properties** file to point to the alternative binaries:
::
  binary.prosite.pfscanv3.path=${bin.directory}/prosite/altbin/pfscanV3.noaf
  binary.prosite.pfsearchv3.path=${bin.directory}/prosite/altbin/pfsearchV3.noaf


If the problem persist, you may need to compile those binaries from `source <https://github.com/sib-swiss/pftools3/>`__,
making sure to build without affinity by compiling with the flag
::
  cmake -DUSE_AFFINITY=OFF ..

4. HMMER errors
~~~~~~~~~~~~~~~~

The HMM libraries provided by some member databases (SUPERFAMILY and SFLD) are not compatible with
newer HMMER versions and an error will occur when those libraries are being indexed by hmmpress version
greater than '3.1b1'. To avoid this issue we recommend using the HMMER binaries bundled with interproscan.


If you encounter errors not listed above,
`please contact us using EMBL EBI's support form <http://www.ebi.ac.uk/support/interproscan>`__.

Contacting us
~~~~~~~~~~~~~
please give us enough background information when you contact us, such as:

- the linux distribution and version
- the InterProScan version
- the java version
- command line used
- the complete error log if possible
