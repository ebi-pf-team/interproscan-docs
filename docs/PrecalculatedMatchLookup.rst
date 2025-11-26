Precalculated Match Lookup Service
==================================

InterProScan uses a lookup service to check whether or not a protein sequence
has been encountered before by InterPro and, therefore, if matches already exist
exist. (see `"How to Run" <HowToRun.html>`__ in our documentation). When InterProScan is
queried with a known sequence, it retrieves the result from the lookup
service and reports the result immediately, thereby reducing compute
requirements and improving performance.

The default ``InterProScan`` configuration will use the lookup
service hosted at EBI http://www.ebi.ac.uk/interpro/match-lookup/version.
This will be will be the most recent lookup service version. Therefore, ``InterProScan`` will 
require access to the internet to run when the using the Match Lookup Service (MLS) is enabled.

Disabling using the Match Lookup Service
----------------------------------------

If you do not wish or are unable to use the InterPro MLS, you can disable looking for 
precalculated matches by including the ``--no-matches-api`` flag in your ``InterProScan``
command:

.. code-block:: bash

    nextflow run interproscan.nf \
        -profile <executor,containerRuntime> \
        --input <path to input fasta file> \
        --datadir <interpro data dir> \
        --no-matches-api

Using a local precalculated match lookup service
------------------------------------------------

The InterPro Matches API provides programmatic access to pre-computed InterProScan 6 
results for every sequence in UniParc. Each UniParc sequence is identified by its 
MD5 hash, which serves as a unique key for fast lookups of its associated InterPro matches.

Follow the latest documentation for setting up a local instance of the InterPro Matches API
from the `Matches API repository <https://github.com/ProteinsWebTeam/interpro-matches-api>`__.

Once you have your local instance of the Matches API running, you can configure ``InterProScan``
to use it by including the ``--matches-api-url`` flag in your ``InterProScan`` command,
followed by the URL of your local Matches API instance. For example:

.. code-block:: bash

    nextflow run interproscan.nf \
        -profile <executor,containerRuntime> \
        --input <path to input fasta file> \
        --datadir <interpro data dir> \
        --matches-api-url http://localhost:5000 
