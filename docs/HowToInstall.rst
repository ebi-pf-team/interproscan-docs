=======================
Installing InterProScan
=======================

Before installing ``InterProScan``, please check you system satisfies the :ref:`Installation requirements`.

To install the ``InterProScan`` 6 software you need to complete the following steps:

1. Install the core ``InterProScan``
2. Retrieve a InterPro release data set
3. (Optional) Install licensed software (MobiDB, SignalP, DeepTMHMM and Phobius)
4. (Optional) Setup a local InterPro Match Lookup Service (MLS)

If the installation is unsuccessful please check the `FAQs <FAQ.html>`_, raise an issue at our 
`GitHub repository <https://github.com/ebi-pf-team/interproscan6/issues>`_, or 
`raise a ticket <https://www.ebi.ac.uk/about/contact/support/interpro>`_ via InterPro.

.. IMPORTANT::
    Due to licensing ``MobiDB``, ``Phobius``, ``SignalP``, and ``DeepTMHMM`` member database analyses 
    are deactivated in ``InterProScan``. To activate these analyses you will need to obtain
    the relevant licenses and files from the respective providers. Please see the 
    `"Installing licsensed members" documentation <InstallingLicensedApps.html>`_ for more information.

[1] Setting up ``InterProScan`` and its dependencies
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Option A: Pull from Docker
--------------------------

1. Use your container runtime to pull the ``interproscan6`` image from Dockerhub:

For example, using Docker:

.. code-block:: bash

    docker pull interproscan6:latest

Using Singularity:

.. code-block:: bash

    singularity pull interproscan6.sif docker://interpro/interproscan6:latest

Using Apptainer:

.. code-block:: bash

    apptainer pull interproscan6.sif docker://interpro/interproscan6:latest

2. Test the installation:

.. code-block:: bash

    $ nextflow run interproscan.nf --help
    # or...
    $ nextflow run interproscan.nf --version

Option B: Install from source
-----------------------------

The ``InterProScan`` code base is available at `GitHub <https://github.com/ebi-pf-team/interproscan6>`__.

1. Clone the GitHub repository

.. code-block:: bash

    git clone https://github.com/ebi-pf-team/interproscan6.git
    cd interproscan6

2. Build the docker image (which automatically installs all dependencies)

.. code-block:: bash

    docker build -t interproscan6 .

3. Test the installation:

.. code-block:: bash

    $ nextflow run interproscan.nf --help
    # or...
    $ nextflow run interproscan.nf --version

Using an alternative container runtime
--------------------------------------

``InterProScan`` was designed to be built and containerised using Docker. At the moment, 
``InterProScan`` also supports using Singularity and Apptainer. You can find more information 
on this in the :ref:`Using Alternative Container Runners` documentation.

[2] Retrieve an InterPro release dataset
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

``InterProScan`` relies on the models that are incorporated into each of the InterPro
member databases. These data are bundled together and can be retrieved using the
following bash commands:

.. code-block:: bash

    # replace interpro-version with the appropriate version number
    INTERPRO_VERSION="102.0"
    curl "https://ftp.ebi.ac.uk/pub/databases/interpro/iprscan/6/$INTERPRO_VERSION/interproscan-data-$INTERPRO_VERSION.tar.gz" \
        --output interproscan-data-<interpro-version>.tar.gz
    tar -pxzf interproscan-data-<interpro-version>.tar.gz
    mv interproscan-data-<interpro-version>/data .
    rm interproscan-data-<interpro-version> -rf
    rm interproscan-data-<interpro-version>.tar.gz

[3] (Optional) Install licensed software
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Due to licensing ``MobiDB``, ``Phobius``, ``SignalP``, and ``DeepTMHMM`` member database analyses 
are deactivated in ``InterProScan`` by default.

To activate these analyses you will need to obtain
the relevant licenses and files from the respective providers. Please see 
:ref:`Installing Licensed Applications` for more information.

[4] (Optional) Setup a local InterPro Match Lookup Service (MLS)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

``InterProScan``  uses the InterPro Match Lookup Service (MLS) to retrieve pre-calculated matches,
reducing the need for compute on your server and speeding up the
response time. By default, ``InterProScan``  is configured to 
use the web service hosted at the EBI, therefore, your servers will need to have external 
access to http://www.ebi.ac.uk to use it.

If you do not wish to use the InterPro MLS in your analyses then include the 
``--disable_precalc`` flag in your ``InterProScan`` commands to skip checking for 
pre-calculated matches.

Alternatively, you can install a local copy of the MLS. 
The uncompressed MLS disk usage comes to more that 1TB, so it is
recommended just to use the default setup.

Please see `Local Precalculated Match Lookup Service <PrecalculatedMatchLookup.html>`__ documentation for more information.
