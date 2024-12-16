.. _profiles-lable:

================
Runtime profiles
================

``InterProScan`` includes  profiles to enable deployment
on a local systems as well as on high performance computer systems using the schedulers
SLURM or LSF, using Docker, Singularity or Apptainer.

For example, to run ``InterProScan`` locally using docker, use the ``local`` and ``docker`` profiles:

.. code-block:: bash

    nextflow run ebi-pf-team/interproscan6 \
        -profile local,docker \
        --input tests/data/test_prot.fa \
        --datadir data

.. ATTENTION::

    Note that the ``-profile`` option uses a single dash ('-') not a double ('--') dash,
    because it is a built in Nextflow option.

Owing to differences in permissions, architectures, and preferences, you may need
to adapt these existing profiles (located in ``utilities/profiles``) or create your own.

You will need to create your own profiles to run on alternative systems (e.g. Azure or AWSBash), or
use alternate container run times.

If you create your own profile, include the profile in the ``nextflow.config`` file.

If you are unsure how to deploy Nextflow on your system please contact your sysadmin.

You can find more on Nextflow profiles in the 
`Nextflow documentation <https://www.nextflow.io/docs/latest/config.html#config-profiles>`_.
