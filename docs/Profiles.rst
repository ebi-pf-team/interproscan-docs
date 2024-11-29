.. _profiles-lable:

================
Runtime profiles
================

``InterProScan`` includes several profiles to enable deployment 
on a local systems as well as on high performance computer systems. 

Profiles are a set of runtime configuration attributes that can be chosen 
when executing ``InterProScan`` using the ``-profile`` option.

Multiple profiles can be specified by separating the profile names with a comma, for example, 
to run ``InterProScan`` locally using docker, use the ``local`` and ``docker`` profiles:

.. code-block:: bash

    nextflow run ebi-pf-team/interproscan6 \
        -profile local,docker \
        --input tests/data/test_prot.fa \
        --datadir data

.. ATTENTION::

    Note that the ``-profile`` option uses a single dash ('-') not a double ('--') dash,
    because it is a built in Nextflow option.

Profiles
--------

We provide generic profiles for running with Docker, Singularity, and Apptainer container runtimes,
respectively:

* ``docker``
* ``singularity``
* ``apptainer``

We also provide generic profiles for running ``InterProScan`` locally, using the ``local`` profile, and
generic profiles for running with the SLURM and LSF schedulers, using the ``slurm`` and ``lsf`` profiles,
respectively.singularity.

* ``local`` - designed for a local set up, does not define memory, cpu or time requirements, and allows Nextflow to use what it can
* ``slurm`` - a generic slurm profile, defines memory and time requirements, and some limits on retrying failed jobs
* ``lsf`` - a generic lsf profile, define memory and time requirements, and some limits on retrying failed jobs

Owing to differences in permissions, architectures, and preferences, you may need
to adapt these existing profiles (located in ``utilities/profiles``) or create your own.
If you create your own profile, include the profile in the ``nextflow.config`` file.

If you are unsure how to deploy Nextflow on your system please contact your sysadmin.

You can find more on Nextflow profiles in the 
`Nextflow documentation <https://www.nextflow.io/docs/latest/config.html#config-profiles>`_.
