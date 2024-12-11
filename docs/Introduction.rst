Introduction
============

What is InterProScan?
~~~~~~~~~~~~~~~~~~~~~

`InterPro <http://www.ebi.ac.uk/interpro/>`__ is a database that
integrates predictive information about proteins' functions from
a number of partner resources, providing an overview of the families that a
protein belongs to and the domains and sites it contains.

Users that have nucleotide or protein sequences that they wish to 
functionally characterise can use the software package ``InterProScan``. This 
software runs the scanning algorithms from the InterPro databases against
all sequences submitted in FASTA format. Matches are calculated 
against all required member database signatures, and the results 
are then outputted in a variety of formats.

Supported platforms
~~~~~~~~~~~~~~~~~~~

``InterProScan`` version 6 uses the `Nextflow <https://www.nextflow.io/docs>`__ 
workflow manager, and can deployed on a system running 64-bit Linux, Windows or MacOS and
 integrated into HPC schedulers and cloud providers.

To install and run InterProScan
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

1. Install NextFlow (version >= 24.10), please see the `Nextflow documentation <https://www.nextflow.io/>`__

2. Download InterPro data files

.. code-block:: bash

   curl -OJ https://ftp.ebi.ac.uk/pub/software/unix/iprscan/6/102.0/interproscan-data-102.0.tar.gz
   tar -pxzf interproscan-data-102.0.tar.gz

2. Run

.. code-block:: bash

    nextflow run ebi-pf-team/interproscan6 \
      -profile <executor, containerRuntime> \
      --input <path to input FASTA> \
      --datadir interproscan-data-102.0

Built in executors: ``local``, ``slurm``, ``lsf``
Built in container runtimes: ``docker``, ``singularity``, ``apptainer``

.. NOTE::
    The ``--datadir``` flag is not needed when only running member databases that do not require additional data files.
    This only applies to ``mobidblite`` and ``coils``` (which do not require additional datafiles) and the
    licensed software (``SignalP``, ``Phobius``, and ``TMHMM```).

For using alternative executors (e.g. Azure and AWS Batch) and container runtimes (e.g. Podman) please
see the `profiles page <Profiles.html>`__.

For setting up a local installation ``InterProScan`` please see the
`how to install <HowToInstall.html>`__ page.

For more information about using ``InterProScan`` please see the page links
on the right, for example `how to run <HowToRun.html>`__.

Included analyses
~~~~~~~~~~~~~~~~~

This distribution of InterProScan includes:

- `AntiFam <https://academic.oup.com/database/article/doi/10.1093/database/bas003/431613?login=true>`__
- `CDD <http://www.ncbi.nlm.nih.gov/Structure/cdd/cdd.shtml>`__
- `COILS <http://www.ch.embnet.org/software/COILS_form.html>`__
- `FunFam <https://bmcbioinformatics.biomedcentral.com/articles/10.1186/s12859-019-2988-x>`__
- `Gene3D <http://gene3d.biochem.ucl.ac.uk/Gene3D/>`__
- `HAMAP <http://hamap.expasy.org/>`__
- `MOBIDB <http://mobidb.bio.unipd.it/>`__
- `NCBIFAM <https://www.ncbi.nlm.nih.gov/genome/annotation_prok/evidence/>`__
  (including the previous `TIGRFAM <http://www.jcvi.org/cgi-bin/tigrfams/index.cgi>`__ analysis)
- `PANTHER <http://www.pantherdb.org/>`__
- `Pfam <http://pfam.sanger.ac.uk/>`__
- `PIRSF <http://pir.georgetown.edu/pirwww/dbinfo/pirsf.shtml>`__
- `PIRSR <https://www.uniprot.org/help/pir_rules>`__
- `PRINTS <http://www.bioinf.manchester.ac.uk/dbbrowser/PRINTS/index.php>`__
- `PROSITE <http://prosite.expasy.org/>`__ (Profiles and Patterns)
- `SFLD <http://sfld.rbvi.ucsf.edu/django/>`__
- `SMART <http://smart.embl-heidelberg.de/>`__ (unlicensed components only)
- `SUPERFAMILY <http://supfam.cs.bris.ac.uk/SUPERFAMILY/>`__

A number of other analyses are available in ``InterProScan```. These
analyses use licensed components provided by third parties. If you
wish to run these analyses it will be necessary for you to obtain a
license from the vendor and configure your local ``InterProScan```
installation to use these:

- `Phobius <http://phobius.sbc.su.se/>`__
- `SignalP <http://www.cbs.dtu.dk/services/SignalP/>`__
- `DeepTMHMM <https://www.biorxiv.org/content/10.1101/2022.04.08.487609v1>`__

The InterPro team would like to thank the developers and maintainers of
all of these analyses for their valued and on-going support!

.. WARNING::
  The implementation of SMART in ``InterProscan`` uses unlicensed components only. 
  This analysis has simplified post-processing that includes
  an E-value filter, therefore, you should not expect it to give the same
  match output as the fully licensed version of SMART.
