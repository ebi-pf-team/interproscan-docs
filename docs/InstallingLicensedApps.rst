================================
Installing Licensed Applications
================================

Due to licensing ``Phobius``, ``SignalP``, and ``DeepTMHMM`` analyses
are deactivated in InterProScan by default. To activate these analyses you will need to obtain 
the relevant licenses and files from the respective providers. This page walks 
you through setting up the licensed software.

Installing DeepTMHMM
~~~~~~~~~~~~~~~~~~~~

``DeepTMHMM`` uses deep learning methods to predict the membrane topology of transmembrane proteins.

1. Acquire a licensed for `DeepTMHMM <https://services.healthtech.dtu.dk/services/DeepTMHMM-1.0/>`__ and download DeepTMHMM
2. Unpack the ZIP file

.. code-block:: bash

    unzip DeepTMHMM-Academic-License-v1.0.zip <TMHMM-DIR>

3. Define the TMHMM dir path.

If you have a local installation of ``InterProScan6``, update the TMHMM ``dir`` path in
``conf/applications.config``:

.. code-block:: groovy

    tmhmm {
        name = "tmhmm"
        dir = ""   <----- update the dir path
    }

Otherwise, provide your own configuration using the ``-C`` flag, which defines ``params.appsConfig`` from
`conf/applications.conf <https://github.com/ebi-pf-team/interproscan6/blob/main/conf/applications.config>`__
and includes a definition for TMHMM as laid out above.

4. Run ``InterProScan6`` with (Deep)TMHMM

When the ``dir`` field is populated TMHMM will be included in the default applications when the ``--applications``
flag is not used, else include ``tmhmm`` when using the ``--application`` flag:

.. code-block:: bash

    nextflow run ebi-pf-team/interproscan6 \
        -profile <local,slurm,lsf...docker,singularity,apptainer> \
        --input <fasta-file> \
        --datadir <interpro-data-dir> \
        --applications tmhmm

Installing Phobius
~~~~~~~~~~~~~~~~~~

``Phobius`` is a bioinformatic tool for the prediction of signal peptides and 
transmembrane domains in protein sequences.

1. Obtain a license for ``Phobius`` and download a copy of the code from the `Phobius server <https://software.sbc.su.se/phobius.html>`_.
2. Unpack the downloaded ``tar`` file into your desired directory

.. code-block:: bash

    tar -xzf phobius101_linux.tgz -C <PHOBIUS-DIR>

3. Define the ``dir`` path for Phobius.

If you have a local installation of ``InterProScan6``, update the ``Phobius`` ``dir`` path in
``conf/applications.config``:

.. code-block:: groovy

    phobius {
        name = "Phobius"
        invalid_chars = "-*.OXUZJ"
        dir = ""    <----- Add relative or absolute path here
    }

Otherwise, provide your own configuration using the ``-C`` flag, which defines ``params.appsConfig`` from
`conf/applications.conf <https://github.com/ebi-pf-team/interproscan6/blob/main/conf/applications.config>`__
and includes a definition for ``Phobius``` as laid out above.

4. Run ``InterProScan6`` with ``Phobius``

When the ``dir`` field is populated ``Phobius`` will be included in the default applications when the ``--applications``
flag is not used, else include ``phobius`` when using the ``--application`` flag:

.. code-block:: bash

    nextflow run ebi-pf-team/interproscan6 \
        -profile <local,slurm,lsf...docker,singularity,apptainer> \
        --input <fasta-file> \
        --datadir <interpro-data-dir> \
        --applications phobius

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

Alternatively, create your own configuration file and include ``SignalP``, and pass this to ``InterProScan6``:

.. code-block:: groovy

    nextflow run ebi-pf-team/interproscan6 \
          -c <path-to-config-file.config> \
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

Run with GPU acceleration
-------------------------

The model weights that come with the ``SignalP`` installation by default run on your CPU.
If you have a GPU available, you can convert your installation to use the GPU instead. 

You will need to install ``SignalP`` in order to convert to GPU models.

1. Convert the ``SignalP`` installation to GPU by following the `SignalP documentation <https://github.com/fteufel/signalp-6.0/blob/main/installation_instructions.md#converting-to-gpu>`_.
2. Run ``InterProScan6`` with the flag ``--signalpGPU``.

.. code-block:: bash

    nextflow run ebi-pf-team/interproscan6 \
          -profile <docker/singularity/apptainer...local/lsf/slurm> \
          --input <FASTA> \
          --datadir <DATADIR> \
          --signalpGPU
