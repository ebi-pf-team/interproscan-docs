=======================
Installing InterProScan
=======================

Before installing ``InterProScan``, please check you system satisfies the :ref:`Installation requirements`.

To install the ``InterProScan6`` in brief:

1. Set up ``InterProScan`` and run the built-in test
2. (Optional) Install licensed software (SignalP, DeepTMHMM and Phobius)
3. (Optional) Setup a local InterPro Match Lookup Service (MLS)

``InterProScan6`` automatically pulls down all necessary containers using the specified container runtime,
otherwise it will run on bare metal.

If the installation is unsuccessful please check the `FAQs <FAQ.html>`_, raise an issue at our
`GitHub repository <https://github.com/ebi-pf-team/interproscan6/issues>`_, or
`raise a ticket <https://www.ebi.ac.uk/about/contact/support/interpro>`_ via InterPro.

[1] Set up InterProScan
~~~~~~~~~~~~~~~~~~~~~~~

Option A: No set up
-------------------

Run ``InterProScan6`` using:

.. code-block:: bash

    nextflow run ebi-pf-team/interproscan6 \
      -profile <executor, containerRuntime> \
      --input <path to input FASTA> \
      --datadir <path to the downloaded InterPro data dir>

``InterProScan6`` supports using Docker, Singularity and Apptainer, on Linux, MacOS, Windows,
SLURM and LSF when using this method. To use an alternative scheduler or container runtime you will need
to set up a local installation.

Option B: Install from source
-----------------------------

Download the ``InterProScan`` software

.. code-block:: bash

    # using git
    git clone https://github.com/ebi-pf-team/interproscan6.git
    # alternatively using wget
    wget -o https://github.com/ebi-pf-team/interproscan6/archive/refs/heads/main.zip


Run ``InterProScan6`` using:

.. code-block:: bash

    nextflow run ebi-pf-team/interproscan6 \
      -profile <executor, containerRuntime> \
      --input <path to input FASTA> \
      --datadir <path to the downloaded InterPro data dir>

Test the installation
---------------------

Test the installation using the provided test profile. For example, to run the test locally using Docker:

.. code-block:: bash

    nextflow run ebi-pf-team/interproscan6 \
      -profile docker,test \
      --datadir data

[2] (Optional) Install licensed software
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Due to licensing ``Phobius``, ``SignalP``, and ``DeepTMHMM`` member database analyses 
are deactivated in ``InterProScan`` by default. To activate these analyses you will need to obtain
the relevant licenses and files from the respective providers. Please see 
:ref:`Installing Licensed Applications` for more information.

[3] (Optional) Setup a local InterPro Match Lookup Service (MLS)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

``InterProScan``  uses the InterPro Match Lookup Service (MLS) to retrieve pre-calculated matches,
thus reducing the total runtime. By default, ``InterProScan``  is configured to
use the web service hosted at the EBI, therefore, your servers will need to have external 
access to http://www.ebi.ac.uk to use it.

If you do not wish to use the InterPro MLS in your analyses then include the 
``--no-matches-api`` flag in your ``InterProScan`` commands.

Alternatively, you can install a local copy of the MLS. 
The uncompressed MLS disk usage comes to more that 1TB, so it is
recommended just to use the default setup.

Please see `Local Precalculated Match Lookup Service <PrecalculatedMatchLookup.html>`__ documentation for more information.
