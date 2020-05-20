Activating Phobius/SignalP/TMHMM analyses
-----------------------------------------

By default the Phobius, SignalP and TMHMM member database analyses are
deactivated because they contain licensed components. In order to
activate these analyses please obtain the relevant license and files
from the provider (ensuring the software version numbers are the same as
those supported by your current InterProScan installation).

An example of how to activate the Phobius 1.01, SignalP 4.1 and TMHMM
2.0 analyses with InterProScan 5.19-58.0 is given below. Files can be
placed in any location as long as your interproscan.properties
configuration is updated accordingly.

Phobius
~~~~~~~

Website: http://phobius.sbc.su.se/data.html

Files required by InterProScan:

-  bin/phobius/1.01/decodeanhmm
-  bin/phobius/1.01/phobius.model
-  bin/phobius/1.01/phobius.options
-  bin/phobius/1.01/phobius.pl

Example inteproscan.properties configuration:

::

    phobius.signature.library.release=1.01
    binary.phobius.pl.path=bin/phobius/1.01/phobius.pl

SignalP
~~~~~~~

Website: http://www.cbs.dtu.dk/services/SignalP/

For academic users there is a download site at:
http://www.cbs.dtu.dk/cgi-bin/nph-sw\_request?signalp Other users are
requested to contact software@cbs.dtu.dk.

Files required by InterProScan:

-  bin/signalp/4.1/signalp
-  bin/signalp/4.1/bin/nnhowplayer.Linux\_i386
-  bin/signalp/4.1/bin/nnhowplayer.Linux\_i486
-  bin/signalp/4.1/bin/nnhowplayer.Linux\_i586
-  bin/signalp/4.1/bin/nnhowplayer.Linux\_i686
-  bin/signalp/4.1/bin/nnhowplayer.Linux\_ia64
-  bin/signalp/4.1/bin/nnhowplayer.Linux\_x86\_64

Example inteproscan.properties configuration:

::

    signalp_euk.signature.library.release=4.1
    signalp_gram_positive.signature.library.release=4.1
    signalp_gram_negative.signature.library.release=4.1
    binary.signalp.path=bin/signalp/4.1/signalp
    signalp.perl.library.dir=bin/signalp/4.1/lib

Please confirm that the following line in the "signalp" binary is set to
the required location:

::

    BEGIN {
        $ENV{SIGNALP} = 'bin/signalp/4.1';
    }

TMHMM
~~~~~

Website: http://www.cbs.dtu.dk/services/TMHMM/

There is a download page
http://www.cbs.dtu.dk/cgi-bin/nph-sw\_request?tmhmm for academic users;
other users are requested to contact CBS Software Package Manager at
software@cbs.dtu.dk.

Files required by InterProScan:

-  bin/tmhmm/2.0c/decodeanhmm
-  data/tmhmm/2.0c/TMHMM2.0c.model

Example inteproscan.properties configuration:

::

    tmhmm.signature.library.release=2.0c
    binary.tmhmm.path=bin/tmhmm/2.0c/decodeanhmm

    tmhmm.model.path=data/tmhmm/2.0c/TMHMM2.0c.model

