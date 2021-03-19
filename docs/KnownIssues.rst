Known issues
============

Open issues in InterProScan
---------------------------

This page documents the latest list of known issues, and we are working to fix them
as soon as possible. For assistance with other InterProScan problems,
`please contact us using EMBL EBI's support form <http://www.ebi.ac.uk/support/interproscan>`__.

1. Error after more than '90% completed'
----------------------------------------

On some file systems, at the end of the analysis, for example, after InterProScan displays more than 90% of the analysis has been completed, you get a non specific error like below:
::
  12/03/2021 15:44:00:000 76% completed
  12/03/2021 15:45:12:110 90% completed
  2021-03-12 15:45:45,201 [amqEmbeddedWorkerJmsContainer-6] [uk.ac.ebi.interpro.scan.jms.worker.LocalJobQueueListener:213] 
             ERROR - Execution thrown when attempting to executeInTransaction the StepExecution.  
             All database activity rolled back.

If you running InterProScan-5.48-83.0 or InterProScan-5.50-84.0, then download a patch as described below: 
::
  cd <interproscan_install_dir>  
  wget ftp://ftp.ebi.ac.uk/pub/databases/interpro/iprscan/5/5.50-84.0/patch/interproscan-5.50-patch.zip
  unzip -o  interproscan-5.50-patch.zip

If this fix fails to solve the problem, do `contact us <#contacting-us>`__. 

2. CDD/RPSBlast errors.
~~~~~~~~~~~~~~~~~~~~~~~

On some linux systems, you may get rpsblast errors like
::
  bin/blast/ncbi-blast-2.10.1+/rpsbproc: error while loading shared libraries: libgomp.so.1: cannot open shared object file: No such file or directory

The missing library is libgomp1. On Ubuntu you might install it as follows:
::
  sudo apt-get install -y libgomp1

On other systems, you have simmilar installation commands

3. Coils errors
~~~~~~~~~~~~~~~~
If you see an error concerning **Coils**, for example the error below, it means the binary
we provide is not compartible with your system.
::

  Cannot run program ".../bin/ncoils/2.2.1/ncoils": error=2, No such file or directory


In this case, you may need to compile the **Coils** binary and it is straight forward as follows.
::
  cd src/coils/ncoils/2.2.1
  make
  cd ../../../..
  cp src/coils/ncoils/2.2.1/ncoils bin/ncoils/2.2.1/ncoils

These steps should update the Coils binary.


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
