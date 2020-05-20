Change log for InterProScan JSON output format
==============================================

InterProScan 5.31-70.0
~~~~~~~~~~~~~~~~~~~~~~

In InterProScan 5.30-69.0 a MobiDB Lite match would consist of the
following output:

::

    {
        "signature" : {
          "accession" : "mobidb-lite",
          "name" : "disorder_prediction",
          "description" : "consensus disorder prediction",
          "type" : null,
          "signatureLibraryRelease" : {
            "library" : "MOBIDB_LITE",
            "version" : "1.5"
          },
          "entry" : null
        },
        "locations" : [ {
          "start" : 1508,
          "end" : 1530,
        } ],
        "model-ac" : "mobidb-lite"
      }

Changes in InterProScan 5.31-70.0 have the following impact on the JSON
output:

1. A 'location' will always consist of one or more 'location-fragments'
   [applies to all analyses]. Most locations will only have one
   fragment, however multiple fragments are possible in Pfam,
   CATH-Gene3D and SUPERFAMILY analyses where discontinuous domains are
   present.

A 'location-fragment' will contain a start and stop position, and a
'dc-status' to indicate whether it is: \* CONTINUOUS (a continuous
single chain domain) \* N\_TERMINAL\_DISC (N-terminal discontinuous) \*
C\_TERMINAL\_DISC (C-terminal discontinuous) \* NC\_TERMINAL\_DISC (N
and C-terminal discontinuous)

2. A 'location' may have an optional 'sequence-feature' [only applies to
   MobiDB Lite locations].

3. The 'type' was removed from the 'signature' as these were never
   populated [applies to all analyses].

4. For a HMMER3 based 'location', a 'postProcessed' boolean attribute
   now indicates whether the native HMMER3 output was subject to
   analysis specific post-processing [applies to HMMER3 based analyses
   only].

Example new output:

::

    {
        "signature" : {
          "accession" : "mobidb-lite",
          "name" : "disorder_prediction",
          "description" : "consensus disorder prediction",
          "signatureLibraryRelease" : {
            "library" : "MOBIDB_LITE",
            "version" : "2.0"
          },
          "entry" : null
        },
        "locations" : [ {
          "start" : 1508,
          "end" : 1530,
          "sequence-feature" : "Polyampholyte",
          "location-fragments" : [ {
            "start" : 1508,
            "end" : 1530,
            "dc-status" : "CONTINUOUS"
          } ]
        } ],
        "model-ac" : "mobidb-lite"
      }
