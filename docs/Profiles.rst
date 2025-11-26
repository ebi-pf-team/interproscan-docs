.. _profiles-lable:

================
Runtime profiles
================

The profiles in ``InterProScan6`` define the time and resource allocations for the analyses.
We recommend reviewing the relevant profile configuration files in ``utilities/profiles``
to ensure they met requirements and expected practices of your system.
If you are unsure how to deploy Nextflow on your system contact the sysadmin.
You can find out more information on the ``InterProScan`` profiles `here <Profiles.html>`__. Please
refer to this documentation before creating your own profiles.

In ``InterProScan``, the built in profiles are used to define the executor (e.g. SLURM, LSF) 
and container runtime (e.g. Docker, Singularity, Apptainer).

For example, to run ``InterProScan`` locally using docker, use the ``docker`` profile:

.. code-block:: bash

    nextflow run ebi-pf-team/interproscan6 \
        -profile docker\
        --input tests/data/test_prot.fa \
        --datadir data

You will need to create your own profiles to run on alternative systems (e.g. Azure or AWSBash), or
use alternate container run times.

If you create your own profile, include the profile in the ``nextflow.config`` file.

You can find more on Nextflow profiles in the 
`Nextflow documentation <https://www.nextflow.io/docs/latest/config.html#config-profiles>`_.
