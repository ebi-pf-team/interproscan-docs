Installation requirements
=========================

InterProScan is developed to run on Linux. There are no versions planned
for Windows or Apple (MAC OS X) operating systems. This is due to
constraints in the various third-party binaries that InterProScan runs.

Note that InterProScan and the individual member database analyses are
processor and memory intensive.

A minimum specification requirement is a machine with 2 cores and 4 GB
of RAM, which will allow the analysis of a small number of sequences at
a time. However the more resources the faster the analysis/more
sequences can be analysed at a time.

Software requirements:

-  64-bit Linux
-  Perl 5 (default on most Linux distributions)
-  Python 3.8 (InterProScan 5.70-102.0 onwards)
-  Java JDK/JRE version 11 (InterProScan 5.37-76.0 onwards)
-  Environment variables set

   -  $JAVA\_HOME should point to the location of the JVM
   -  $JAVA\_HOME/bin should be added to the $PATH

How to check these on a system?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Which version of Linux am I running?
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

InterProScan has been prepared with 64-bit binaries. To determine if you
have a 32-bit or a 64-bit system, enter on the command line:

::

    uname -a

The exact response will depend upon the hardware vendor & architecture,
however typical responses may look like:

**64-bit** as hinted by x86\_64

::

    $ uname -a
    Linux bob.com 2.6.32-358.6.2.el6.x86_64 #1 SMP Tue May 14 15:48:21 EDT 2013 x86_64 x86_64 x86_64 GNU/Linux

**32-bit** as hinted by i686

::

    $ uname -a
    Linux jim.com 2.6.32-50-generic-pae #112-Ubuntu SMP Tue Jul 9 20:44:31 UTC 2013 i686 GNU/Linux

If you are still in any doubt, ask your systems administrator.

Testing your Perl installation
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

To test that Perl 5 is installed, enter on the command line

::

    perl -version

This should report a version of Perl is available, similar to:

::

    This is perl, v5.10.1 (*) built for i486-linux-gnu-thread-multi

    Copyright 1987-2009, Larry Wall

    ...etc

A default Perl installation is sufficient: no third party Perl modules
need to be installed.

Alternatively you could change the value of the 'perl.command' property
in your interproscan.properties configuration file to point at a
suitable Perl installation, the default value is:

::

    perl.command=perl

Testing your Python installation
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

To test that Python 3 is installed, enter on the command line

::

    python3 --version

This should report a version of Python is available, similar to:

::

    Python 3.11.6

A default Python installation is sufficient: no third party Python
modules need to be installed.

You could also change the value of the 'python3.command' property in
your interproscan.properties configuration file to point at a suitable
Python installation, the default value is:

::

    python3.command=python3

Testing the Java environment
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

To test your environment, enter on the command line

::

    java -version

This should report a version of java is available, similar to:

::

    openjdk version "11.0.4" 2019-07-16
    OpenJDK Runtime Environment AdoptOpenJDK (build 11.0.4+11)
    OpenJDK 64-Bit Server VM AdoptOpenJDK (build 11.0.4+11, mixed mode)


**InterProScan release 5.37-76.0  or later will only run with Java
version 11.*++  **

You can get Java from many places. We have tested Java 11 from the OpenJDK Binaries from https://adoptopenjdk.net/
You can get information on OpenJDK reference implementations at https://jdk.java.net/ and download from https://openjdk.java.net/install/index.html

InterProScan releases prior to 5.37-76.0 required Java 8.

Appendix - Historical Java version testing information
''''''''''''''''''''''''''''''''''''''''''''''''''''''

Any Oracle/Open JDK/JRE with Java 1.11.x should work with InterProScan.
Historical information about Java versions tested and confirmed to
work/not work include below for information but this is not an
exhaustive list!

**Oracle JDK/JRE** for InterProScan 5.17-56.0 or later

+---------------+-----------------+------------------------+--------------------+----------------+
| **Version**   | **Build**       | **Operating System**   | **Architecture**   | **Status**     |
+===============+=================+========================+====================+================+
| 1.8.074       | 1.8.0\_74-b02   | Linux                  | x64                | Works          |
+---------------+-----------------+------------------------+--------------------+----------------+
| 1.8.060       | 1.8.0\_60-b27   | Linux                  | x86                | Works          |
+---------------+-----------------+------------------------+--------------------+----------------+
| 1.7.\*        | -               | Linux                  | x86                | Doesn't work   |
+---------------+-----------------+------------------------+--------------------+----------------+

**OpenJDK** for Interproscan 5.17-56.0 or later

+---------------+------------------------+--------------------+----------------+--------+
| **Version**   | **Operating System**   | **Architecture**   | **Status**     | Misc   |
+===============+========================+====================+================+========+
| 1.8.0\_66     | Linux                  | x64                | Works          |        |
+---------------+------------------------+--------------------+----------------+--------+
| 1.7.\*        | Linux                  | x64                | Doesn't work   |        |
+---------------+------------------------+--------------------+----------------+--------+

**Oracle JDK/JRE** for InterProScan 5.16-55.0 or before

+---------------+-----------------+------------------------+--------------------+----------------+
| **Version**   | **Build**       | **Operating System**   | **Architecture**   | **Status**     |
+===============+=================+========================+====================+================+
| 1.8.0         | 1.8.0-Works     | Linux                  | x64                | Doesn't work   |
+---------------+-----------------+------------------------+--------------------+----------------+
| 1.7.0\_51     | 1.7.0\_51-b13   | Linux                  | x86                | Works          |
+---------------+-----------------+------------------------+--------------------+----------------+
| 1.7.0\_40     | -               | Linux                  | x64                | Works          |
+---------------+-----------------+------------------------+--------------------+----------------+
| 1.7.0         | -               | Linux                  | x64                | Works          |
+---------------+-----------------+------------------------+--------------------+----------------+
| 1.6.0\_45     | -               | Linux                  | x64                | Works          |
+---------------+-----------------+------------------------+--------------------+----------------+
| 1.6.0\_37     | -               | Linux                  | x64                | Works          |
+---------------+-----------------+------------------------+--------------------+----------------+
| 1.6.0\_22     | -               | Linux                  | x64                | Works          |
+---------------+-----------------+------------------------+--------------------+----------------+
| 1.6.0\_11     | -               | Linux                  | x64                | Works          |
+---------------+-----------------+------------------------+--------------------+----------------+
| 1.6.0\_07     | -               | Linux                  | x64                | Works          |
+---------------+-----------------+------------------------+--------------------+----------------+
| 1.6.0\_05     | -               | Linux                  | x64                | Works          |
+---------------+-----------------+------------------------+--------------------+----------------+
| 1.6.0\_04     | -               | Linux                  | x64                | Works          |
+---------------+-----------------+------------------------+--------------------+----------------+
| 1.6.0\_03     | -               | Linux                  | amd64              | Doesn't work   |
+---------------+-----------------+------------------------+--------------------+----------------+
| 1.6.0\_02     | -               | Linux                  | amd64              | Doesn't work   |
+---------------+-----------------+------------------------+--------------------+----------------+

**OpenJDK** for InterProScan 5.16-55.0 or before

+---------------+--------------------------------+--------------------+----------------+--------------------+
| **Version**   | **Operating System**           | **Architecture**   | **Status**     | Misc               |
+===============+================================+====================+================+====================+
| 1.7.0\_25     | Linux                          | x64                | Works          | :---               |
+---------------+--------------------------------+--------------------+----------------+--------------------+
| 1.6.0\_30     | Linux                          | i686               | Works          | :---               |
+---------------+--------------------------------+--------------------+----------------+--------------------+
| 1.6.0\_27     | Linux                          | x64                | Works          | :---               |
+---------------+--------------------------------+--------------------+----------------+--------------------+
| 1.6.0\_24     | Linux (Red Hat Distribution)   | x64                | Doesn't work   | Reported by user   |
+---------------+--------------------------------+--------------------+----------------+--------------------+
