=======================
Installing InterProScan
=======================

Before installing ``InterProScan``, please check you system satisfies the :ref:`Installation requirements`.

To install the ``InterProScan6`` software you need to complete the following steps:

1. Retrieve a InterPro release data set
2. Install Nextflow and a container runtime
3. Set up ``InterProScan``
4. (Optional) Install licensed software (SignalP, DeepTMHMM and Phobius)
5. (Optional) Setup a local InterPro Match Lookup Service (MLS)

If the installation is unsuccessful please check the `FAQs <FAQ.html>`_, raise an issue at our 
`GitHub repository <https://github.com/ebi-pf-team/interproscan6/issues>`_, or 
`raise a ticket <https://www.ebi.ac.uk/about/contact/support/interpro>`_ via InterPro.

.. IMPORTANT::
    Due to licensing ``Phobius``, ``SignalP``, and ``DeepTMHMM`` member database analyses
    are deactivated in ``InterProScan``. To activate these analyses you will need to obtain
    the relevant licenses and files from the respective providers. Please see the 
    `"Installing licsensed members" documentation <InstallingLicensedApps.html>`_ for more information.


[1] Retrieve an InterPro release dataset
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

``InterProScan`` relies on the models that are incorporated into each of the InterPro
member databases. These data are bundled together and can be retrieved using the
following bash commands:

.. code-block:: bash

    # replace interpro-version with the appropriate version number
    INTERPRO_VERSION="103.0"
    curl "https://ftp.ebi.ac.uk/pub/databases/interpro/iprscan/6/$INTERPRO_VERSION/interproscan-data-$INTERPRO_VERSION.tar.gz" \
        --output interproscan-data-<interpro-version>.tar.gz
    tar -pxzf interproscan-data-<interpro-version>.tar.gz
    mv interproscan-data-<interpro-version>/data .
    rm interproscan-data-<interpro-version> -rf
    rm interproscan-data-<interpro-version>.tar.gz

[2] Install Nextflow
~~~~~~~~~~~~~~~~~~~~

Install all `Nextflow requirements <https://www.nextflow.io/docs/latest/install.html#requirements>`__.
Then install Nextflow using the `Nextflow documentation <https://www.nextflow.io/docs/latest/install.html>`__,
or install via `Conda <https://anaconda.org/bioconda/nextflow>`__ or `Pypi <https://pypi.org/project/nextflow/>`__.

[3] Set up InterProScan
~~~~~~~~~~~~~~~~~~~~~~~

Nextflow can use your container runtime to pull down the ``InterProScan6`` container
from DockerHub, therefore, removing the need to manually retrieve the container or set up a local installation.
Alternatively, you can manually pull down the ``InterProScan6`` image from docker hub, or set up a local installation.

Option A: The no install option
-------------------------------

Nextflow can pull the ``InterProScan6``` pipeline directly from DockerHub without requiring installation.
To do this, use the ``ebi-pf-team/interproscan6``` image by running the following command:

.. code-block:: bash

    nextflow run ebi-pf-team/interproscan6 \
      -profile <executor, containerRuntime> \
      --input <path to input FASTA> \
      --datadir <path to the downloaded InterPro data dir>

``InterProScan6`` supports using Docker, Singularity and Apptainer, on Linux, MacOS, Windows,
SLURM and LSF when using this method. To use an alternative scheduler or container runtime you will need
to set up a local installation.

Option B: Pull from DockerHub manually
--------------------------------------

Alternatively, you can pull the ``InterProScan`` image yourself from DockerHub using your container
runtime of choice.

For example, using Docker:

.. code-block:: bash

    docker pull ebi-pf-team/interproscan6:latest

Using Singularity:

.. code-block:: bash

    singularity pull interproscan6.sif docker://ebi-pf-team/interproscan6:latest

Using Apptainer:

.. code-block:: bash

    apptainer pull interproscan6.sif docker://ebi-pf-team/interproscan6:latest

If you run ``InterProScan6`` using Singularity or Apptainer and do not want to set up local installation of ``InterProScan6``,
please ensure the ``interproscan6.sif`` container images are in your current working directory (because this
is where ``InterProScan6`` looks for these files by default).

To use local container images with alternative names, or which are not in the current working directory, please set up
a local installation of ``InterProScan6``, and update the container image path in the corresponding
(``<container>.conf``) file in ``utilities/profiles``. You can find more information on this in
the :ref:`Using Alternative Container Runners` documentation.

Option C: Install from source
-----------------------------

The ``InterProScan`` code base is available at `GitHub <https://github.com/ebi-pf-team/interproscan6>`__.

1. Download the ``InterProScan`` software

.. code-block:: bash

    git clone https://github.com/ebi-pf-team/interproscan6.git
    cd interproscan6

Or if you do not have ``git``:

.. code-block:: bash

    wget -o https://github.com/ebi-pf-team/interproscan6/archive/refs/heads/main.zip

2. Pull down the docker image or build the docker image.

To build the docker image (which automatically installs all dependencies)

.. code-block:: bash

    docker build -t interproscan6 .

3. Test the installation:

.. code-block:: bash

    $ nextflow run main.nf --help

You can find more information on this in the :ref:`Using Alternative Container Runners` documentation.

[3] (Optional) Install licensed software
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Due to licensing ``Phobius``, ``SignalP``, and ``DeepTMHMM`` member database analyses 
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
