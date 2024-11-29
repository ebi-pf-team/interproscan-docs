================================
Installing Licensed Applications
================================

Due to licensing ``Phobius``, ``SignalP``, and ``DeepTMHMM`` analyses
are deactivated in InterProScan by default. To activate these analyses you will need to obtain 
the relevant licenses and files from the respective providers. This page walks 
you through setting up the licensed software.

The installation instructions below use the Docker.
To use alternative container runtimes please see the `Using Alternative Container Runtimes page <AlternativeContainers.html>`__
of the this documentation.

.. important::

    Using licensed software requires a local installation of ``InterProScan6``.

Installing DeepTMHMM
~~~~~~~~~~~~~~~~~~~~

``DeepTMHMM`` uses deep learning methods to predict the membrane topology of transmembrane proteins.

Coming soon...

Installing Phobius
~~~~~~~~~~~~~~~~~~

``Phobius`` is a bioinformatic tool for the prediction of signal peptides and 
transmembrane domains in protein sequences.

1. Obtain a license for ``Phobius`` and download a copy of the code from the `Phobius server <https://software.sbc.su.se/phobius.html>`_.
2. Unpack the downloaded ``tar`` file into your desired directory

.. code-block:: bash

    tar -xzf phobius101_linux.tgz -C <PHOBIUS-DIR>

3. Copy the Docker file from ``InterProScan`` ``utilities/docker/Phobius/Dockerfile`` to your local ``Phobius`` directory

.. code-block:: bash

    # with the terminal pointed at the root of this repo
    cp docker_files/phobius/Dockerfile <PHOBIUS-DIR>/Dockerfile

4. Build a Docker image

.. code-block:: bash

    # with the terminal pointed at your local phobius dir
    docker image build -t phobius .

5. Add ``Phobius`` to the ``conf/applications.config`` file with the path to your local ``Phobius`` directory.

.. code-block:: groovy

    phobius {
            name = "Phobius"
            invalid_chars = "-*.OXUZJ"
            dir = ""    <----- Add relative or absolute path here
        }

Alternatively, create your own configuration file and include ``Phobius``, and pass this to ``InterProScan6``:

.. code-block:: groovy

    nextflow run ebi-pf-team/interproscan6 \
          -c <Path-to-config-file.config> \
          -profile <docker/singularity/apptainer...local/lsf/slurm> \
          --input <FASTA> \
          --datadir <DATADIR>

6. (Optional) Convert the Docker image to an image of your container runtime.

For example, to build a singularity image:

.. code-block:: bash

    docker save phobius > phobius.tar
    singularity build phobius.sif docker-archive://phobius.tar

Installing SignalP
~~~~~~~~~~~~~~~~~~

``SignalP`` is a bioinformatic tool for the prediction of signal peptides in protein sequences.

1. Obtain a license of `SignalP <https://services.healthtech.dtu.dk/services/SignalP-6.0/>`_
2. Download ``SignalP6`` from the `SignalP6 server <https://services.healthtech.dtu.dk/services/SignalP-6.0/>`_ (under 'Downloads').

.. NOTE::
    Either fast or slow models can be implemented. To change the implemented mode 
    please see the :ref: `Changing-mode` documentation below

3. Unpackage the ``SignalP6`` ``tar`` file

.. code-block:: bash

    tar -xzf signalp-6.0h.fast.tar.gz -C <SIGNALP-DIR>

4. Update the path to the unpackaged SignalP tar file in the ``conf/applications.config`` configuration file:

.. code-block:: groovy

        signalp_euk {
            name = "SignalP-Euk"
            organism = "eukarya"
            dir = ""            <---- update the path to unpackaged SignalP tar file
            mode = "fast"
        }
        signalp_prok {
            name = "SignalP-Prok"
            organism = "other"
            dir = ""            <---- update the path to unpackaged SignalP tar file
            mode = "fast"
        }

Alternatively, create your own configuration file and include ``SignalP`, and pass this to ``InterProScan6``:

.. code-block:: groovy

    nextflow run ebi-pf-team/interproscan6 \
          -c <Path-to-config-file.config> \
          -profile <docker/singularity/apptainer...local/lsf/slurm> \
          --input <FASTA> \
          --datadir <DATADIR>

Using ``signalp_prok`` sets the ``organism`` argument for SignalP6 to 'other', configuring ``SignalP6``
to run using all models.

Using ``signalp_euk`` sets the ``organism`` argument for SignalP6 to 
'eukaryotic', limiting the prediction to to the Sec/SPI models, change the ``organism`` value to 
``"eukaryote"`` or ``"euk"``. As stated in the ``SignalP6`` 
`documentation <https://github.com/fteufel/signalp-6.0/blob/main/installation_instructions.md>`_:
"Specifying the eukarya method of ``SignalP6`` (``SignalP_EUK``) triggers post-processing of 
the SP predictions by ``SignalP6`` to prevent spurious results (only predicts type Sec/SPI)."

Changing mode
-------------

``SignalP6`` supports 3 modes: ``fast``, ``slow`` and ``slow-sequential``. 
To change the mode of ``SignalP6``:

1. Incorporate the new mode into your ``SignalP6`` installation as per the `SignalP6 documentation <https://github.com/fteufel/signalp-6.0/blob/main/installation_instructions.md#installing-additional-modes>`_.

2. Use the ``--signalpMode`` flag when running ``InterProScan`` to define the mode.

.. code-block:: bash

    nextflow run ebi-pf-team/interproscan6 \
          -profile <docker/singularity/apptainer...local/lsf/slurm> \
          --input <FASTA> \
          --datadir <DATADIR> \
          --signalpMode slow

``InterProScan`` supports running only **one** ``SignalP`` mode at a time.

.. WARNING::
    The slow mode can take 6x longer to compute. Use when accurate region borders are needed.

Converting from CPU to GPU
--------------------------

The model weights that come with the ``SignalP`` installation by default run on your CPU.
If you have a GPU available, you can convert your installation to use the GPU instead. 

You will need to install ``SignalP`` in order to convert to GPU models.

1. Convert the ``SignalP`` installation to GPU by following the `SignalP documentation <https://github.com/fteufel/signalp-6.0/blob/main/installation_instructions.md#converting-to-gpu>`_.
2. Build a Docker image using the GPU-compatible models (using the Docker file provided in ``InterProScan``)

.. code-block:: bash

    # with the terminal pointed at your local signalp dir
    docker image build -t signalp6_gpu .

3. (Optional) Convert the image to your container runtime of choice

4. Run ``InterProScan6`` with the flag ``--signalpGPU``.

.. code-block:: bash

    nextflow run ebi-pf-team/interproscan6 \
          -profile <docker/singularity/apptainer...local/lsf/slurm> \
          --input <FASTA> \
          --datadir <DATADIR> \
          --signalpGPU
