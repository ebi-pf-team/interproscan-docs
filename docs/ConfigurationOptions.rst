Configuration Options
=====================

This page will give you an overview and a detailed description about
some of the available configuration options in your InterProScan 5
properties file (**interproscan.properties**).

+---------------+--------------------+------------------------+
| **Option**    | **Description**    | **Default setting**    |
+===============+====================+========================+
| **Precalculat |                    |                        |
| ed            |                    |                        |
| match lookup  |                    |                        |
| and proxy     |                    |                        |
| setup**       |                    |                        |
+---------------+--------------------+------------------------+
| precalculated | Host name of your  | Not set                |
| .match.lookup | proxy (e.g.        |                        |
| .service.prox | http://proxy.examp |                        |
| y.host        | le.ebi.ac.uk).     |                        |
|               | **You** would need |                        |
|               | to set that        |                        |
|               | option, if the     |                        |
|               | pre-calculated     |                        |
|               | match lookup       |                        |
|               | service is enabled |                        |
|               | and you have a     |                        |
|               | proxy              |                        |
|               | (communication     |                        |
|               | layer) between you |                        |
|               | and the world wide |                        |
|               | web. **Please      |                        |
|               | note** user        |                        |
|               | proxy-authenticati |                        |
|               | on                 |                        |
|               | is not supported   |                        |
|               | at the moment.     |                        |
+---------------+--------------------+------------------------+
| precalculated | Open port of your  | Not set                |
| .match.lookup | proxy (e.g. 8080)  |                        |
| .service.prox |                    |                        |
| y.port        |                    |                        |
+---------------+--------------------+------------------------+
| precalculated | Web address of the |`http://www.ebi.ac.uk/in|
| .match.lookup | precalculated      |terpro/match-lookup`    |
| .service.url  | match lookup       |                        |
|               | service. Used if   |                        |
|               | the pre-calculated |                        |
|               | match lookup       |                        |
|               | service is         |                        |
|               | enabled. **You**   |                        |
|               | would only want to |                        |
|               | change that, if    |                        |
|               | you have installed |                        |
|               | a local version of |                        |
|               | the `lookup        |                        |
|               | service <LocalLook |                        |
|               | upService.html>`__ |                        |
+---------------+--------------------+------------------------+
| **Other       |                    |                        |
| properties**  |                    |                        |
+---------------+--------------------+------------------------+
| exclude.sites | Calculate residue  | false                  |
| .from.output  | level annotation   |                        |
|               | and include in the |                        |
|               | output where       |                        |
|               | available?         |                        |
+---------------+--------------------+------------------------+
