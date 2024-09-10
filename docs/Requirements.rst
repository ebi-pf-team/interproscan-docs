Installation requirements
=========================

OS System:
~~~~~~~~~~

``InterProScan`` and all its third party tools are run using the Nextflow workflow system 
and containers, thus allowing the program to be deployed on Linux, Windows and MacOS systems.

System requirements:
~~~~~~~~~~~~~~~~~~~~

Please note that ``InterProScan`` and the individual member database analyses are
processor and memory intensive.

The minimum specification requirement is a machine with 2 cores and 6 GB
of RAM, which will allow the analysis of a small number of sequences at
a time. However the more resources the faster the analysis will be, and the more
sequences can be analysed at a time.

Software requirements:
~~~~~~~~~~~~~~~~~~~~~~

- Nextflow (version >=23.10)
    - Install using the `Nextflow documentation <https://www.nextflow.io/docs/latest/install.html>`__
    - Or install via `Conda <https://anaconda.org/bioconda/nextflow>`__ or `Pypi <https://pypi.org/project/nextflow/>`__
-  Java JDK/JRE version >=11
    -  Environment variables set
        -  ``$JAVA\_HOME`` should point to the location of the JVM
        -  ``$JAVA\_HOME/bin`` should be added to the ``$PATH``
- Container runtime
    - ``InterProScan`` includes built in support for Docker, Singularity or Apptainer
    - Using alternative containter runtimes will require creating your `Nextflow profiles <Profiles.html>`__

.. TIP::
    You can find tips on installing Java in the `Nextflow documentation <https://www.nextflow.io/docs/latest/install.html>`__.

Testing the Java environment
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

To test your environment, enter on the command line

.. code-block:: bash

    java -version

This should report a version of java is available, similar to:

.. code-block:: bash

    openjdk version "11.0.4" 2019-07-16
    OpenJDK Runtime Environment AdoptOpenJDK (build 11.0.4+11)
    OpenJDK 64-Bit Server VM AdoptOpenJDK (build 11.0.4+11, mixed mode)
