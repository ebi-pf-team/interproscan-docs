================================
Installing Licensed Applications
================================

Due to licensing ``Phobius``, ``SignalP``, and ``DeepTMHMM`` analyses
are deactivated in InterProScan by default. To activate these analyses you will need to obtain 
the relevant licenses and files from the respective providers.

For each of these member databases:

1. Retrieve the relevant license and download the analysis software:

* ``DeepTMHMM``: Contact the DeepTMHMM help desk and request a stand-alone licensed copy of DeepTMHMM.
* ``Phobius``: From the `Phobius server <https://software.sbc.su.se/phobius.html>`__
* ``SignalP``: From the `SignalP6 server <https://services.healthtech.dtu.dk/services/SignalP-6.0/>`__ (under 'Downloads')

2. Unpack the ``ZIP`` or ``TAR`` file

.. code-block:: bash
    # deeptmhmm
    unzip DeepTMHMM-Academic-License-v1.0.zip <DEEPTMHMM-DIR>
    # phobius
    tar -xzf phobius101_linux.tgz -C <PHOBIUS-DIR>
    # signalp
    tar -xzf signalp-6.0h.fast.tar.gz -C <SIGNALP-DIR>

3. Update the relevant member's ``dir`` path in ``conf/applications.con`` or provide your own configuration using
the ``-C`` flag, which defines ``params.appsConfig`` from
`conf/applications.conf <https://github.com/ebi-pf-team/interproscan6/blob/main/conf/applications.config>`__
and includes a definition for the member as laid out in ``conf/applications.com``:

.. code-block:: groovy

    deeptmhmm {
        name = "tmhmm"
        dir = "<DEEPTMHMM-DIR>"    <---- update the dir path
        has_data=false
    }
    phobius {
        name = "Phobius"
        invalid_chars = "-*.OXUZJ"
        dir = "<PHOBIUS-DIR>"      <---- update dir path
        has_data=false
    }
    signalp_euk {
        name = "SignalP-Euk"
        organism = "eukarya"
        dir = "<SIGNALP-DIR>"      <---- update dir path
        mode = "fast"
        has_data=false
    }
    signalp_prok {
        name = "SignalP-Prok"
        organism = "other"
        dir = "<SIGNALP-DIR>"      <---- update dir path
        mode = "fast"
        hase_data=false
    }

4. Run. When the ``dir`` field is populated for each member they will be included in the default applications when
the ``--applications`` flag is not used, else include the member when using the ``--application`` flag.

.. code-block:: bash

    nextflow run ebi-pf-team/interproscan6 \
        -profile <local,slurm,lsf...docker,singularity,apptainer> \
        --input <fasta-file> \
        --applications deeptmhmm,phobius,signalp_euk,signalp_prok

SignalP Prokayte versus Eukaryote
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Using ``signalp_prok`` sets the ``organism`` argument for SignalP6 to 'other', configuring ``SignalP6``
to run using all models.

Using ``signalp_euk`` sets the ``organism`` argument for SignalP6 to 
'eukaryotic', limiting the prediction to to the Sec/SPI models, change the ``organism`` value to 
``"eukaryote"`` or ``"euk"``. As stated in the ``SignalP6`` 
`documentation <https://github.com/fteufel/signalp-6.0/blob/main/installation_instructions.md>`_:
"Specifying the eukarya method of ``SignalP6`` (``SignalP_EUK``) triggers post-processing of 
the SP predictions by ``SignalP6`` to prevent spurious results (only predicts type Sec/SPI)."

SignalP - Changing mode
~~~~~~~~~~~~~~~~~~~~~~~

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

Run SignalP with GPU acceleration
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

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
