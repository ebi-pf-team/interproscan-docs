.. _index:

InterProScan documentation
===========================

``InterProScan`` is a software tool that allows users to analyse novel nucleotide or 
protein sequences by running them against the `InterPro <http://www.ebi.ac.uk/interpro/>`__ 
database. InterPro is a resource that integrates functional information about proteins 
from a number of member databases, such as the families they belong to and the domains 
and sites they contain.

With ``InterProScan``, users can submit their protein or nucleic acid sequences in 
FASTA format and the tool will match them against the various databases integrated 
within InterPro. The results are then outputted in multiple formats, providing users 
with an overview of the functional characteristics of their sequences.

``InterProScan`` version 6 uses the `Nextflow <https://www.nextflow.io/docs>`__ 
workflow system for deployment. This means ``InterProScan`` 
can be deployed on a system running 64-bit Linux, Windows or MacOS. Additionally, 
Nextflow enables the integration of ``InterProScan`` into HPC schedulers and cloud providers.

.. toctree::
   :maxdepth: 3
   :caption: Contents:

   Introduction
   ReleaseNotes
   Requirements
   HowToInstall
   InstallingLicensedApps
   HowToRun
   Profiles
   InputFormat
   OutputFormats
   ScanNucleicAcidSeqs
   PrecalculatedMatchLookup
   UnderTheHood
   Benchmark
   ImprovingPerformance
   FAQ
   Citing
   Feedback
