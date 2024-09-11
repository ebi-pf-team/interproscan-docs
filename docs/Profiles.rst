.. _profiles-lable:

================
Runtime profiles
================

``InterProScan`` includes several profiles to allow automated scaling for deployment 
on a local systems as well as on high performance computer systems. 

Profiles are a set of runtime configuration attributes that can be chosen 
when executing ``InterProScan`` using the ``-profile`` option.

Multiple profiles can be specified by separating the profile names with a comma, for example, 
to run ``InterProScan`` locally using docker, use the ``local`` and ``docker`` profiles:

.. code-block:: bash

    nextflow run interproscan.nf \
        -profile local,docker \
        --input utilities/test_files/best_to_test.fasta

.. ATTENTION::

    Note that the ``-profile`` option uses a single dash ('-') not a double ('--') dash,
    because it is a built in Nextflow option.

Profiles
--------

We provide generic profiles or running with Docker, Singularity, and Apptainer container runtimes, 
respectively:

* ``docker``
* ``singularity``
* ``apptainer``

We also provide generic profiles for running ``InterProScan`` locally, using the ``local`` profile;
a generic profile for running with the SLURM scheduler, using the ``slurm`` profile; and a profile 
specifically for running on the EBI cluster, using the ``ebi`` profile:

* ``local`` - designed for a local set up, does not define memory, cpu or time requirements, and allows Nextflow to use what it can
* ``slurm`` - a generic slurm profile, defines memory and time requirements, and some limits on retrying failed jobs
* ``lsf`` - a generic lsf profile, define memory and time requirements, and some limits on retrying failed jobs
* ``ebi`` - a profile for running ``InterProScan`` on a HPC with the SLURM scheduler, define memory and time requirements, as well as partition configuration options which are allowed on the EBI cluster but may not be supported or approved on all cluster farms.

.. TIP:: 
    You do not need to specify the ``local`` profile when running ``InterProScan`` locally, you need 
    only specify the container runtime, but it is recommended to help with managing resources.

Owing to differences in permissions, architectures, resources and preferences, you may need 
to adapt these existing profiles (located in ``utilities/profiles``) or create your own.
If you create your own profile, add the profile as a new configuration file in 
``utilities/profiles`` and include the profile in the ``nextflow.config`` file.

If you are unsure how to deploy Nextflow on your specific system of choice contact the sysadmin.

You can find more on Nextflow profiles in the 
`Nextflow documentation <https://www.nextflow.io/docs/latest/config.html#config-profiles>`_.
