=================================================
Integrating InterProScan into a Nextflow pipeline
=================================================

You can integrate ``InterProScan`` directly into you own Nextflow pipelines.

For example using Git ``submodules``:

1. Add the submodule to a 'subworkflows' directory:

.. code-block:: bash

    git submodule add \
        --depth 1 https://github.com/ebi-pf-team/interproscan6.git \
        subworkflows/interproscan6

2. Checkout the 6.0.0-beta release inside the submodule:

.. code-block:: bash

    cd subworkflows/interproscan6
    git fetch --depth 1 origin tag 6.0.0-beta
    git checkout 6.0.0-beta
    cd ../..

3. Record the checked-out commit

.. code-block:: bash

    git add subworkflows/interproscan6
    git commit -m "Added interproscan6 6.0.0-beta submodule"

4. Copy the ``InterProScan`` library into your own projects library

.. code-block:: bash

    cp -r subworkflows/interproscan6/lib* lib

5. If you are running on baremetal skip to step 4, otherwise, add the containers definitions from the corresponding ``InterProScan`` profile (e.g. ``conf/profiles/docker.config``) to your Nextflow config file (``nextflow.config``).

6. Include the ``PREPARE_INTERPROSCAN`` and ``RUN_INTERPROSCAN`` processes in your own Nextflow pipeline script.

.. code-block:: groovy

    include { PREPARE_INTERPROSCAN; INTERPROSCAN } from './subworkflows/interproscan6/workflows/interproscan.nf'

    import java.nio.file.*

    workflow {
        fasta_file = Channel.fromPath("input-fasta.faa")               // channel for the input FASTA file
        applications = ["cdd", "coils", "mobidblite", "pfam", "smart"] // list of InterProScan analyses to run
        apps_config = "subworkflows/interproscan6/conf/applications.config" // path to applications configuration file
        data_dir = Paths.get("data").toRealPath()                      // [path] InterProScan data directory
        Files.createDirectories(data_dir)                              // ensure data directory exists
        output = "results/integrated-ips6"  // [str] Path and output filename prefix. Output dirs need to already exist.
        formats = ["tsv", "gff3", "xml"]    // [list] output formats to generate
        interpro_version = "latest"         // [str] InterPro version label or tag to use
        interproscan_version = "6.0.0"      // [str] InterProScan software version
        interproscan_name = "InterProScan6" // [str] human-readable name for this InterProScan instance
        no_matches_api = false              // [bool] when true, do not query the InterPro matches API
        matches_api_url = "https://www.ebi.ac.uk/interpro/matches/api" // [str] URL for the InterPro matches API
        matches_api_chunk_size = 100        // [int] number of sequences per API chunk request
        matches_api_max_retries = 3         // [int] maximum retries for API calls on failure
        batch_size = 5000                   // [int] number of sequences per processing batch
        sub_batch_size = 1000               // [int] number of sequences per sub-batch within a batch
        nucleic = false                     // [bool] when true, treat input sequences as nucleic acids
        skip_interpro = false               // [bool] when true, skip the InterPro annotation step
        goterms = true                      // [bool] include GO term mappings in the output
        pathways = true                     // [bool] include pathway mappings in the output
        globus = false                      // [bool] when true, enable Globus transfer of results
        use_gpu = false                     // [bool] when true, enable GPU acceleration if supported

        PREPARE_INTERPROSCAN(apps_config, applications, use_gpu)
        apps_config = PREPARE_INTERPROSCAN.out.apps_config.val  // make sure to extract the value
        
        output_files = INTERPROSCAN(
            fasta_file,
            applications,
            apps_config,
            data_dir,
            output,
            formats,
            interpro_version,
            interproscan_version,
            interproscan_name,
            no_matches_api,
            matches_api_url,
            matches_api_chunk_size,
            matches_api_max_retries,
            batch_size,
            sub_batch_size,
            nucleic,
            skip_interpro,
            goterms,
            pathways,
            globus
        )

        output_files.view { "Generated output file: ${it}" }
    }
