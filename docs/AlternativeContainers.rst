===================================
Using Alternative Container Runners
===================================

In addition to supporting Docker, ``InterProScan`` provides built in support for container runners Apptainer and 
Singularity.

Nextflow also includes support for Charliecloud, Podman, Sarus and Shifter. To use these container runtimes you will need to create a new Nextflow Profile. 
Please see the `Profiles page <Profiles.html>`__ for more information.

.. TIP::

    It is not necessary to download the InterPro release data before building any 
    container for ``InterProScan``.

Apptainer
---------

`Apptainer <https://apptainer.org/>`_ is an open source fork of Singularity, and a 
popular alternative to Docker.
Apptainer does not require root privileges or a separate daemon process, unlike Docker.

You will need Apptainer installed on the system(s) you will use to build the container image
and run the pipeline.

Build an Apptainer image
~~~~~~~~~~~~~~~~~~~~~~~~

You can pull the ``interproscan6`` image from Docker Hub using Apptainer:

.. code-block:: bash

    apptainer pull interproscan6.sif docker://interpro/interproscan6:latest    

Alternatively, you can convert the ``interproscan6`` docker image on your host system, then export and transfer 
the resulting Apptainer image to another system. This approach may be required for 
`installing licensed software <InstallingLicensedApps.html>`__.

1. Save an existing Docker image to a tar file.
2. Build an Apptainer image from the tar file.

.. code-block:: bash

    docker save interproscan6 > interproscan6.tar
    singularity build interproscan6.sif docker-archive://interproscan6.tar

.. code-block:: bash

    docker save phobius > phobius.tar
    singularity build phobius.sif docker-archive://phobius.tar

Keep the Apptainer image (``interproscan6.sif``) in the root of the ``InterProScan`` repository 
on the system you will use to run the pipeline. Otherwise, you will 
need to update the paths to the Apptainer image in the ``utilities/profiles/apptainer.config`` profile.

Running with Apptainer
~~~~~~~~~~~~~~~~~~~~~~

When running ``InterProScan`` with Apptainer include "apptainer" in the ``-profile`` option.
E.g.

.. code-block:: bash

    nextflow run interproscan.nf \
        --input my_seqs.fasta \
        -profiles apptainer

Singularity
-----------

`Singularity <https://sylabs.io/singularity/>`_ is a popular alternative to Docker.
Singularity does not require root privileges or a separate daemon process, unlike Docker.

You will need SingularityCE or SingularityPro installed on the system(s) 
you will use to build the container image and run the pipeline.

.. NOTE::

    ``InterProScan`` does not differentiate between SingularityCE (an open source 
    Singularity supported by Sylabs) and SingularityPro (commercial software from Sylabs). 
    Nextflow implements a generic "singularity" executor for both.

Build a Singularity image
~~~~~~~~~~~~~~~~~~~~~~~~~~

You can pull the ``interproscan6`` image from Docker Hub using Singularity:

.. code-block:: bash

    singularity pull interproscan6.sif docker://interpro/interproscan6:latest    

Alternatively, you can convert the ``interproscan6`` docker image on your host system, then export and transfer 
the resulting Singularity image to another system. This approach may be required for 
`installing licensed software <InstallingLicensedApps.html>`__.

1. Build or pull the Docker image.
2. Save the Docker image to a ``tar`` archive.
3. Build a Singularity image from the ``tar`` archive.

For example, for the ``interproscan6`` image:

.. code-block:: bash

    docker build -t interproscan6 .
    docker save interproscan6 > interproscan6.tar
    singularity build interproscan6.img docker-archive://interproscan6.tar

Keep the Singularity image (``interproscan6.img``) in the root of the ``InterProScan`` repository 
on the system you will use to run the pipeline. Otherwise, you will 
need to update the paths to the Singularity image in the ``utilities/profiles/singularity.config`` profile.

Running with Singularity
~~~~~~~~~~~~~~~~~~~~~~~~

When running ``InterProScan`` with Singularity include "singularity" in the ``-profile`` option.
E.g.

.. code-block:: bash

    nextflow run interproscan.nf \
        --input my_seqs.fasta \
        -profiles singularity

You can find more information on the ``InterProScan`` profiles :ref: `profiles-lable`.
