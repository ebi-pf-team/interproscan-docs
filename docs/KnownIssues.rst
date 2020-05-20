Known issues
============

This page documents the latest list of known issues to be aware of. For
assistance with other InterProScan problems, please contact us.

Open issues in InterProScan
---------------------------

We are aware of the following issues, which will be fixed as soon as
possible:

CDD/RPSBlast errors on older or new systems.
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

On older linux systems or latest releases, you may get rpsblast errors
like
``/usr/lib64/libstdc++.so.6: version `GLIBCXX_3.4.15' not found (required by bin/...)``

For CDD/RPSBlast you will have two choices: \* download the
ncbi-blast-2.6.0+ tarball from NCBI and extract rpsblast and copy to
bin/blast/ncbi-blast-2.6.0+/ OR \* follow the instructions on how to
compile rpsblast/rpsbproc as found here:
https://github.com/ebi-pf-team/interproscan/wiki/CompilingBinaries#cdds-rpsblastrpsbproc

If you continue to get errors even after compiling the necessary
binaries for your system as documented in
https://github.com/ebi-pf-team/interproscan/wiki/CompilingBinaries send
us a message http://www.ebi.ac.uk/support/interpro-general-query

Authenticated proxy servers are not supported.
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

At the moment proxy servers requiring authentication are not supported.

FingerPRINTScan errors on older systems.
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

On older linux systems, you may get FingerPRINTScan run errors. To solve
this you should follow the instructions on
https://github.com/ebi-pf-team/interproscan/wiki/CompilingBinaries#fingerprintscan

Translation stops are not stripped off
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Translation stops/asterix at the end of a protein sequence are not
stripped off.

Problems building InterProScan from source
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

We are aware that on some Ubuntu machines, some of the tests in
uk.ac.ebi.interpro.scan.persistence package fail when you compile
InterProScan. This can be avoided by switching off all unit tests:

::

    mvn -Dmaven.test.skip=true clean install
