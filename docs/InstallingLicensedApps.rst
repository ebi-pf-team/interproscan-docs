================================
Installing Licensed Applications
================================

Due to licensing ``MobiDB``, ``Phobius``, ``SignalP``, and ``DeepTMHMM`` analyses 
are deactivated in InterProScan by default. To activate these analyses you will need to obtain 
the relevant licenses and files from the respective providers. This page walks 
you through setting up the licensed software.

.. NOTE::
    The installation instructions below use the Docker files provided within the 
    ``InterProScan`` code base (available via `GitHub <https://github.com/ebi-pf-team/interproscan6/tree/main>`_).
    To use alternative container runtimes please see the `Using Alternative Container Runtimes page <AlternativeContainers.html>`__
    of the this documentation.

Installing DeepTMHMM
~~~~~~~~~~~~~~~~~~~~

``TMHMM`` is used to predict the presence of transmembrane domains within protein sequences. 
``DeepTMHMM`` specifically uses deep learning methods to predict the membrane topology of 
transmembrane proteins. The model employed by DeepTMHMM encodes the primary amino acid sequence 
by using a pre-trained language model and decodes the topology by using a state space model 
in order to produce topology and type predictions.

Coming soon... (temporarily held due to licensing).

Installing MobiDB / MobiDB-Lite
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

MobiDB-Lite provides fast consensus annotation of intrinsic disorder regions in proteins.

Some components of `MoiDB-Lite <https://github.com/BioComputingUP/MobiDB-lite>`_ are GPL-licensed. 
When you include GPL-licensed software in your own software, you need to license your software in GPL - 
this would include ``InterProScan`` and any work generated using ``InterProScan``. To avoid this, 
EBI provides a GNU-license implementation of the software used to screen for matches against 
the MobiDB-Lite database, called `idrpred <https://github.com/matthiasblum/idrpred>`_.

Pull the ``idrpred`` image from Docker Hub using your container runtime of choice:

For example, using Docker:

.. code-block:: bash

    docker pull matblum/idrpred:latest

Using Singularity:

.. code-block:: bash

    singularity pull idrpred.sif docker://matblum/idrpred:latest

Using Apptainer:

.. code-block:: bash

    apptainer pull idrpred.sif docker://matblum/idrpred:latest

Installing Phobius
~~~~~~~~~~~~~~~~~~

``Phobius`` is a bioinformatic tool for the prediction of signal peptides and 
transmembrane domains in protein sequences.

1. Obtain a license for ``Phobius`` and download a copy of the code from the `Phobius server <https://software.sbc.su.se/phobius.html>`_.
2. Unpack the downloaded ``tar`` file into your desired directory

.. code-block:: bash

    tar -xzf phobius101_linux.tgz -C <PHOBIUS-DIR>

3. Copy the Docker file from ``InterProScan`` ``docker_files/Phobius/Dockerfile`` to your local ``Phobius`` directory

.. code-block:: bash

    # with the terminal pointed at the root of this repo
    cp docker_files/phobius/Dockerfile <PHOBIUS-DIR>/Dockerfile

4. Build a Docker image

.. code-block:: bash

    # with the terminal pointed at your local phobius dir
    docker image build -t phobius .

5. Check the ```subworkflows/sequence_analysis/members.config`` file to make sure the ``Phobius`` version is correct.

.. code-block:: groovy

    phobius {
            release = "1.01" <---- update if necessary
            runner = "phobius"
        }

6. (Optional) Convert the Docker image to an image of your container runtime.

For example, to build a singularity image:

.. code-block:: bash

    docker save phobius > phobius.tar
    singularity build phobius.sif docker-archive://phobius.tar

Installing SignalP
~~~~~~~~~~~~~~~~~~

``SignalP`` is a bioinformatic tool for the prediction of signal peptides and the location of their 
cleavage sites.

You can find the official ``SignalP`` installation documentation `here <https://github.com/fteufel/signalp-6.0/blob/main/installation_instructions.md>`_.

1. Obtain a license of `SignalP <https://services.healthtech.dtu.dk/services/SignalP-6.0/>`_
2. Download ``SignalP6`` from the `SignalP6 server <https://services.healthtech.dtu.dk/services/SignalP-6.0/>`_ (under 'Downloads').

.. NOTE::
    Either fast or slow models can be implemented. To change the implemented mode 
    please see the :ref: `Changing-mode` documentation below

3. Unpackage the ``SignalP6`` ``tar`` file

.. code-block:: bash

    tar -xzf signalp-6.0h.fast.tar.gz -C <SIGNALP-DIR>

4. Copy the Docker file available in the ``./docker_files/signalp/`` directory of ``InterProScan`` 
to the root of your local ``SignalP6`` directory (``<SIGNALP-DIR>``), so that **all** ``SignalP6`` 
files are mounted when building the Docker image.:

.. code-block:: bash

    # with the terminal point at the root of this repo
    cp docker_files/signalp/Dockerfile <SIGNALP-DIR>/Dockerfile

5. Build a docker image

.. code-block:: bash

    # with the terminal pointed at your local signalp dir
    docker build -t signalp6 .

6. Check the SignalP release number is correct in the ``interproscan/subworkflows/sequence_analysis/members.config`` configuration file:

.. code-block:: groovy

    signalp {
        release = "6.0h"  <--- make sure the release is correct
        runner = "signalp"
        ...
    }
    ...
    signalp_euk {
        release = "6.0h"  <--- make sure the release is correct
        runner = "signalp_euk"
        ...
    }

7. (Optional) Convert the Docker image to an image of your container runtime.

For example, to build a singularity image:

.. code-block:: bash

    docker save signalp6 > signalp6.tar
    singularity build signalp6.sif docker-archive://signalp6.tar

Running SignalP
---------------

Include ``signalp`` or ``signalp_euk`` in the list of applications defined using the ``--applications`` flag.

.. code-block:: bash

    nextflow run interproscan.nf \
        --input utilities/test_files/best_to_test.fasta \
        --applications signalp \
        -profile local,docker


.. code-block:: bash

    nextflow run interproscan.nf \
        --input utilities/test_files/best_to_test.fasta \
        --applications signalp_euk \
        -profile local,docker

Using ``signalp`` sets the ``organism`` argument for SignalP6 to 'other', configuring ``SignalP6`` 
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

2. Use the ``--signalp_mode`` flag when running ``InterProScan`` to define the mode.

For example, to run `InterProScan` with the input file `best_to_test.fasta`, using SignalP with only eukaryotic models in slow mode, and with retrieving precalculated matches disabled on a local machine using docker:

.. code-block:: bash

    nextflow run interproscan.nf \
    --input utilities/test_files/best_to_test.fasta \
    --applications signalp_euk \
    --disable_precalc \
    --signalp_mode slow \
    -profile docker,local

.. NOTE::
    ``InterProScan`` supports running only **one** ``SignalP`` mode at a time.

.. WARNING::
    The slow mode can take 6x longer to compute. Use when accurate region borders are needed.

Converting from CPU to GPU, and back again
------------------------------------------

The model weights that come with the ``SignalP`` installation by default run on your CPU.
If you have a GPU available, you can convert your installation to use the GPU instead. 

You will need to install ``SignalP`` in order to convert to GPU models.

1. Convert the ``SignalP`` installation to GPU by following the `SignalP documentation <https://github.com/fteufel/signalp-6.0/blob/main/installation_instructions.md#converting-to-gpu>`_. Note this requires installing ``SignalP``.
2. Build a Docker image using the GPU-compatible models (using the Docker file provided in ``InterProScan``)

.. code-block:: bash

    # with the terminal pointed at your local signalp dir
    docker image build -t signalp6_gpu .

3. (Optional) Convert the image to your container runtime of choice

For example, to build a singularity image:

.. code-block:: bash

    docker save signalp6_gpuu > signalp6_gpu.tar
    singularity build signalp6_gpu.sif docker-archive://signalp6_gpu.tar

To run ``SignalP`` with GPU acceleration with ``InterProScan6`` use the flag ``--signalp_gpu``.

For example, to run ``InterProScan`` with only ``SignalP`` enabled, using GPU acceleration on a SLURM cluster with Singularity support:

.. code-block:: bash

    nextflow run interproscan.nf \\
    --input <fasta file> \\
    --applications signalp \\
    --signalp_gpu \\
    -profile singularity,slurm

To run ``SignalP`` with the GPU acceleration, include the ``--signalp_gpu`` flag in the
``InterProScan`` command.
