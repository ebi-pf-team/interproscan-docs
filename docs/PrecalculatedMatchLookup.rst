Precalculated match lookup
==========================

InterProScan uses a lookup service to check whether or not a protein
submitted to it has been encountered before and, therefore, if matches
exist. (see "How to Run" in User documentation). This generic mechanism
is based upon a REST web service that retrieves data from a
`BerkeleyDB <http://en.wikipedia.org/wiki/Berkeley_DB>`__ database.

The client to this service is built into the InterProScan 5 software, to
allow lookup from the web service. The web service can be installed and
run "out of the box", using
`Jetty <http://jetty.codehaus.org/jetty/>`__.

The service support two simple queries:

- "Do these sequences need to be analysed?" This query returns protein sequences that have **not**
  been analysed previously. Proteins are considered to have been analysed
  previously even if they have no matches.
    - **Input**: Set of protein sequence MD5 checksums
    - **Output**: MD5 checksums of proteins that have **not** been analysed previously

- "What are the matches for these sequences?"
    - **Input**: Set of protein sequence MD5 checksums
    - **Output**: Simple "BekerkeleyMatchXML" document containing all matches.

Both of these services are used in I5 - the former to ensure that
protein sequences with no matches are not re-analysed needlessly.

Incorporation into InterProScan 5
---------------------------------

The hook into this service is from the
`ProteinLoader <https://github.com/ebi-pf-team/interproscan/tree/master/core/business/src/main/java/uk/ac/ebi/interpro/scan/business/sequence/ProteinLoader.java>`__
class, into which is injected a
`BerkeleyPrecalculatedProteinLookup <https://github.com/ebi-pf-team/interproscan/tree/master/core/business/src/main/java/uk/ac/ebi/interpro/scan/business/sequence/BerkeleyPrecalculatedProteinLookup.java>`__,
which is an implementation of the
`PrecalculatedProteinLookup <https://github.com/ebi-pf-team/interproscan/tree/master/core/business/src/main/java/uk/ac/ebi/interpro/scan/business/sequence/PrecalculatedProteinLookup.java>`__
interface.

A
`MatchHttpClient <https://github.com/ebi-pf-team/interproscan/tree/master/core/precalcmatches/precalc-match-client/src/main/java/uk/ac/ebi/interpro/scan/precalc/client/MatchHttpClient.java>`__
instance is injected into the
`BerkeleyPrecalculatedProteinLookup <https://github.com/ebi-pf-team/interproscan/tree/master/core/business/src/main/java/uk/ac/ebi/interpro/scan/business/sequence/BerkeleyPrecalculatedProteinLookup.java>`__
class, which is used to query the web service. The client is configured
from properties to set the URL of the web service, should users wish to
install the web service locally.

The
`BerkeleyPrecalculatedProteinLookup <https://github.com/ebi-pf-team/interproscan/tree/master/core/business/src/main/java/uk/ac/ebi/interpro/scan/business/sequence/BerkeleyPrecalculatedProteinLookup.java>`__
then uses the client to query for pre-calculated matches / proteins that
have been previously analysed. Complete InterProScan 5 Protein objects
with a set of Matches are returned from the
`BerkeleyPrecalculatedProteinLookup <https://github.com/ebi-pf-team/interproscan/tree/master/core/business/src/main/java/uk/ac/ebi/interpro/scan/business/sequence/BerkeleyPrecalculatedProteinLookup.java>`__
to the
`ProteinLoader <https://github.com/ebi-pf-team/interproscan/tree/master/core/business/src/main/java/uk/ac/ebi/interpro/scan/business/sequence/ProteinLoader.java>`__
instance. The
`ProteinLoader <https://github.com/ebi-pf-team/interproscan/tree/master/core/business/src/main/java/uk/ac/ebi/interpro/scan/business/sequence/ProteinLoader.java>`__
then persists these matches and ensures that the Protein objects
included are **not** scheduled for reanalysis.
