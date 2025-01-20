=========================
Benchmarking InterProScan
=========================

Both Nextflow and IPS6 provide tools to assess the performance of IPS6. Nextflow provides 
methods to generate HTML reports summarising resource usage and an operational timeline. However, 
these reports are limited to representing a single run of IPS6. Consequently, we have 
packaged a simple benchmarking script into IPS6 that can summarise the performance across multiple runs, additionally, 
allowing the customised grouping of data, for example, to assess differences in performance when 
altering the batch size.

IPS6 Benchmarked
~~~~~~~~~~~~~~~~

We have run InterProScan 6 (version 6.0.0) with Nextflow version 24.04.3 on our local cluster, 
using SLURM, against a complete *Homo sapien* genome, 
containing 87,231 protein sequences (`GCA_011064465.2 <https://www.ncbi.nlm.nih.gov/datasets/genome/GCA_011064465.2/>`_ 
from NCBI GenBank).

As of 1st September 2024, there are approximatley 340+ compute nodes (each with 48 physical cores and 340-468 GB RAM) 
comprising 16,000 hyper-threaded CPU cores on our internal cluster. 

The precalculated MLS was also disabled.

Below is the ``InterProScan`` command used:

.. code-block:: bash

    nextflow run interproscan.nf \
        -profile slurm,singularity \
        --input h.sapien.GCA_011064465.2.faa \
        --disable_precalc \
        --applications 'antifam,cdd,coils,gene3d,funfam,hamap,ncbifam,mobidb,panther,pfam,phobius,pirsf,pirsr,prints,prosite_patterns,prosite_profiles,sfld,smart,signalp,superfamily' \
        -with-timeline \
        -with-report \
        --outdir benchmark

Results coming soon.

How to benchmark IPS6
~~~~~~~~~~~~~~~~~~~~~

Using Nextflow
--------------

You can generate a HTML report summarising the CPU usage, memory usage and runtime per process in IPS6 
using Nextflow. Includes ``-with-report [file name].html`` in your IPS6 command. You can find more in the 
`Nextflow documentation <https://www.nextflow.io/docs/latest/tracing.html#execution-report>`_.

Nextflow can also be configured to generate a visual representation of the timeline of a IPS6 run. 
Include ``-with-timeline [file name].html`` in your IPS6 command. You can find more information
in the `Nextflow documentation pages <https://www.nextflow.io/docs/latest/tracing.html#timeline-report>`_

Using the IPS6 benchmarking script
----------------------------------

1. **Set up:** Install third party pakcages listed in ``benchmarking/requirements.txt``.

.. code-block:: bash

    pip install -r benchmarking/requirements.txt


2. **Build Nextflow trace files:** Nextflow can be configured to generate a trace file each time IPS6 is run. Include the following lines in the ``nextflow.config`` file for IPS6:

.. code-block:: groovy

    trace {
        enabled = true
        fields = 'task_id,process,realtime,duration,status,cpus,%cpu,time,memory,%mem,rss,peak_rss,submit,start,complete,queue'
        file = '[file name]'
        overwrite = true   
    }

IPS6 will overwrite any existing file with the same name given for the trace file. So you may need to move 
each trace file to a unique path after each run of IPS6.

You can find more information on the columns that can be included in the trace in the `Nextflow docs <https://www.nextflow.io/docs/latest/tracing.html#trace-report>`_.

As a minimum, the following columns must be included in the trace file for the IPS6 benchmarking scripts to work:

* process
* realtime
* rss
* peak_rss

3. **Update or create a config file:**

Update the ``benchmarking/tracefile.json`` (or create a new JSON file) keyed by the name 
of each group of analyses, e.g. the number of CPU assigned or the batch size used, and 
valued by a list of string representations of paths to the IPS6 trace files.

For example, if you wanted to assess the impace of altering the batch size:

.. code-block:: json

    {
        "500": [
            "benchmarking/24.08.27.batch.500.report.3.tsv",
            "benchmarking/24.08.26.batch.500.report.2.tsv",
            "benchmarking/24.08.26.batch.500.report.1.tsv"
        ],
        "1000": [
            "benchmarking/24.08.27.batch.1000.report.3.tsv",
            "benchmarking/24.08.26.batch.1000.report.2.tsv",
            "benchmarking/24.08.26.batch.1000.report.1.tsv"
        ],
        "5000": [
            "benchmarking/24.08.27.batch.5000.report.3.tsv",
            "benchmarking/24.08.26.batch.5000.report.2.tsv",
            "benchmarking/24.08.26.batch.5000.report.1.tsv"
        ]
    }

.. IMPORTANT::
    The JSON config file must contain at least one group and all IPS6 trace file paths
    must be assigned to a group.


For example, to not split the runs up into separate groups:

.. code-block:: json

    {
        "All": [
            "benchmarking/24.08.27.batch.500.report.3.tsv",
            "benchmarking/24.08.26.batch.500.report.2.tsv",
            "benchmarking/24.08.26.batch.500.report.1.tsv"
            "benchmarking/24.08.27.batch.1000.report.3.tsv",
            "benchmarking/24.08.26.batch.1000.report.2.tsv",
            "benchmarking/24.08.26.batch.1000.report.1.tsv"
        ],
    }

4. **Run benchmarking:**

The only required argument for running the benchmarking is the path to the JSON file listing the paths to 
the trace files: For example:

.. code-block:: bash

    # running from the root of the IPS6 project dir
    python3 benchmarking/benchmark_ips6.py benchmarking/tracefiles.json

Output files from benchmarking
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Unlike the HTML reports generated by the Nextflow report, the IPS6 benchmarking scripts writes the 
images to separate files to be included any subsequent reports and documents.

Each run of ``benchmark_ips6.py`` will produce the following figures (note, references to 'group' refers to the keys in the input JSON file, each key represents a different 'group'):

* ``total_runtime.*`` - Shows the total run time of IPS6 per group in the input JSON file
* ``process_runtime.*`` - Shows the total run time per process in IPS6
* ``process_runtime_piechart.*`` - Shows the percentage of the total runtime contributed by each process
* ``pie_chart_values.csv`` - Contains the data used to build the process_runtime_piechart.* figure. If many processes are included the legends in the pie chart can often overlap. Use this CSV file to plot the pie chart (or alternative chart).
* ``overall_memory_usage.*`` - Plots the overall memory usage per group in the input JSON file
* ``overall_max_memory_usage.*`` - Plots the overall maximum memory used per group in the input JSON file
* ``memory_per_process.*`` - Plots the memory usage per process (and per group if multiple groups are defined in the input JSON file)
* ``max_memory_per_process.*`` - Plots the maximum memory usage per process (and per group if multiple groups are defined in the input JSON file)

Each box and whisker plot is overlaid by a strip plot with each point of the strip plot representing the value from a single run.

Optional arguments for benchmarking
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

You can print the help message using the ``--help`` / ``-h`` flag:

.. code-block:: bash

    # running from the root of the IPS6 project dir
    python3 benchmarking/benchmark_ips6.py --help


**Group name:**  

By default, the benchmarking will label the groupings as 'Groups' on the resulting plot axes and 
legends. You can name the groupings using the ``--group_name`` flag and providing the name you wish 
to be assigned to the axes and legends, e.g. ``--group_name "Batch Sizes"``, or ``--group_name "Number of CPU"``.

**Figure file formats:**  

By default, the resulting figures are only written out in PDF format. Use the ``--format`` flag to list 
the desired file outputs. Accepted outputs: png, pdf, and svg. For example to generate svg and png files use ``--format png,svg``.

**Adjust the figure sizes:**

``benchmark_ips6.py`` does attempt to adjust the figure size automatically based on the amount of 
data, but you can customise the plot size (when the memory and max memory usage) by using the 
``--fig_size`` flag and providing the width and height, defaults start at 10, 5 
(before IPS6 adjusts for the data set size). Provide the numbers as a space separated list, 
for example:

.. code-block:: bash

    # width = 10, height = 5
    python3 benchmarking/benchmark_ips6.py \
        benchmarking/tracefiles.json \
        --group_name "Batch Size" \
        --outdir testing-benchmarking_fig-size \
        --fig_size 10 5 \
        --save_data

**The trace file contains raw data:**  

By default the trace file writes the in human readable format, but can be configured to write 
the raw values. If this is the case, include the ``--raw`` flag in the ``benchmark_ips6.py`` command.

**Output directory:**  

By default, the output figures will be written to the current working directory. To write the files 
to a desired output directory use the ``--outdir`` flag and provide the path for the output dir. 
The scripts will build all necessary parent directories for the output dir.

**Save the data:**  

If you wish to perform further analyses on the data, use the ``--save_data`` flag to configure 
``benchmark_ips6.py`` to write out the dataframe it generates to a CSV file in the output dir.
