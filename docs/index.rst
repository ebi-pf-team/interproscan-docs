.. _index:

InterProScan documentation
===========================

``InterProScan`` is a software tool for analysing protein and nucleotide sequences 
against the `InterPro <http://www.ebi.ac.uk/interpro/>`__ database, which integrates 
functional information from multiple member databases to identify protein families, domains, 
and sites. With ``InterProScan`` users can submit sequences in FASTA format and receive results in multiple 
formats, providing comprehensive insights into their sequences' functional characteristics.

``InterProScan`` version 6 uses the `Nextflow <https://www.nextflow.io/docs>`__ 
workflow system for deployment. This means ``InterProScan`` 
can be deployed on a system running 64-bit Linux, Windows or MacOS. Additionally, 
Nextflow enables the integration of ``InterProScan`` into HPC schedulers and cloud providers.

.. toctree::
   :maxdepth: 2
   :caption: Contents:

   Introduction
   ReleaseNotes
   Requirements
   HowToInstall
   InstallingLicensedApps
   HowToRun
   MigratingToI6
   Profiles
   InputFormat
   OutputFormats
   ScanNucleicAcidSeqs
   Integrating
   PrecalculatedMatchLookup
   ImprovingPerformance
   UnderTheHood
   FAQ
   Citing
   Feedback
