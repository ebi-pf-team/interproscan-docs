The InterProScan Lookup Match Service
==========================================

The InterProScan match lookup service stores pre-calculated InterProScan
results for the sequences in the InterPro database. When InterProScan is
queried with a known sequence, it retrieves the result from the lookup
service and reports the result immediately, thereby reducing compute
requirements and improving performance.

For sequences not in the lookup
service, InterProScan will calculate these from scratch using the
various analyses requested by the user.

The default interproscan.properties configuration will use the lookup
service hosted at EBI http://www.ebi.ac.uk/interpro/match-lookup/version.
This will be will be the most recent lookup service version and only compatible with
the most recent InterProScan release:

::

    precalculated.match.lookup.service.url=http://www.ebi.ac.uk/interpro/match-lookup

    #proxy set up
    precalculated.match.lookup.service.proxy.host=
    precalculated.match.lookup.service.proxy.port=3128

The default lookup service will not be used in the following scenarios (therefore all calculations
are performed locally):

- The version number of the service does not match your version of InterProScan.
- The service cannot be accessed (e.g., for firewall reasons or it is temporarily unavailable).
- You disable the lookup feature. **To disable** the service you could either:

    -  Use the "-dp" command lineoption.
    -  Set the "precalculated.match.lookup.service.url=" property in your interproscan.properties configuration file (to an empty value).

Installing the lookup service locally
-------------------------------------

You can choose to download and install the InterProScan lookup service
locally if required. This offers you several advantages:

- provide control over the version of the lookup service - if you choose to upgrade InterProScan  less frequently than the release cycle, you can ensure that you are using a lookup service that is synchronized with the version of InterProScan that you are running.
- A dedicated service. You will not be competing with other users for access to the service.
- Control over the scale of the service. The service is extremely responsive (a few milliseconds per sequence request) and a single web server will cope with a high load, however if you expect to put the service under a very high load, you may chose to run the service in parallel on multiple machines, potentially with load balancing.
- Run the service behind your firewall for maximum security.

System requirements
-------------------

Because of the very large size of the Berkeley database used by the
Lookup Service, you are recommended to observe the following minimum
requirements:

-  Java 11
-  Recommended minimum 2 cores (processors)
-  4GB RAM (of which > 2GB will consumed by the service when you run
   it)

Obtaining the lookup service
----------------------------

Version 5.71-102.0 of the lookup service is only compatible with version
5.71-102.0 of InterProScan. Instructions below are for installing the
latest version, you can download previous versions of the lookup service
from https://ftp.ebi.ac.uk/pub/software/unix/iprscan/5/.

This service is a very large download! You are **strongly recommended**
to check the md5 checksum (as described below) to ensure that the file
has been downloaded correctly.

::

    # Create and enter a suitable directory
    mkdir i5_lookup_service
    cd i5_lookup_service

    # Download the tarball and the MD5 file.
    wget https://ftp.ebi.ac.uk/pub/software/unix/iprscan/5/5.71-102.0/lookup_service_5.71-102.0.tar.gz
    wget https://ftp.ebi.ac.uk/pub/software/unix/iprscan/5/5.71-102.0/lookup_service_5.71-102.0.tar.gz.md5

    # Recommended checksum to confirm the download was successful:
    md5sum -c lookup_service_5.71-102.0.tar.gz.md5
    # Must return *lookup_service_5.71-102.0.tar.gz: OK*
    # If not - try downloading the file again as it may be a corrupted copy.

(Direct link:
https://ftp.ebi.ac.uk/pub/software/unix/iprscan/5/5.71-102.0/lookup_service_5.71-102.0.tar.gz)

Extract the tarball:

::

    tar -pxvzf lookup_service_5.71-102.0.tar.gz

    # where:
    #     p = preserve the file permissions
    #     x = extract files from an archive
    #     v = verbosely list the files processed
    #     z = filter the archive through gzip
    #     f = use archive file

The service can be run in one of two ways:

Run with graphical user interface (to set port number)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

If you are running it on a machine with a desktop interface and just
want to test the lookup service, a simple user interface is included to
allow you to set the port number to run the service.

Note that in the example below, the memory available to Java has been
set to 8000MB (using the ``-Xmx8000m`` switch). This is recommended as a
good starting value - you may choose to set this higher if the service
will be used heavily. (We have tested it with -Xmx36000m without problems).

::

    cd lookup_service_5.71-102.0
    java -Xmx8000m -jar server-5.71-102.0-jetty-console.war

A new window will open. Set the port number as required and click the
"Start" button to start the web service running.

The initialization of the web service usually takes a while, depending
on the machine you are running it. After successful initialization you
will be forwarded to the 'InterProScan 5 Pre-calculated Match Lookup
Service' landing page within your browser and from now on the lookup
service is ready to be used.

Run "Headless" (no graphical user interface)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

It is most likely that you will want to run the lookup service
"headless", i.e. purely as a command line tool. In this case, the port
number and other options can be passed in on the command line as
follows:

Note that in the example below, the memory available to Java has been
set to 8000MB (using the ``-Xmx8000m`` switch). This is recommended as a
good starting value - you may choose to set this higher if the service
will be used heavily. (We have tested it with -Xmx36000m).

::

    cd lookup_service_5.71-102.0
    java -Xmx8000m -jar server-5.71-102.0-jetty-console.war  [--option=value] [--option=value]

    # Example command:
    # java -Xmx8000m -jar server-5.71-102.0-jetty-console.war --headless --port 8080

Where options include:

::

    Options:
     --sslProxied        - Running behind an SSL proxy
     --port n            - Create an HTTP listener on port n (default 8080)
     --bindAddress addr  - Accept connections only on address addr (default: accept on any address)
     --forwarded         - Set reverse proxy handling using X-Forwarded-For headers
     --contextPath /path - Set context path (default: /)
     --headless          - Don't open graphical console, even if available
     --help              - Print this help message
     --tmpDir /path      - Temporary directory, default is /tmp

Waiting for the lookup service to start
---------------------------------------

The lookup service is very large and could take over an hour to start.
Example output from a successful startup is given below:

::

    $ java -Xmx8000m -jar server-5.71-102.0-jetty-console.war
    10242 [Thread-2] INFO org.simplericity.jettyconsole.DefaultJettyManager - Added web application on path / from war /example/path/to/server-5.71-102.0-jetty-console.war
    10243 [Thread-2] INFO org.simplericity.jettyconsole.DefaultJettyManager - Starting web application on port 8080
    10245 [Thread-2] INFO org.eclipse.jetty.server.Server - jetty-8.1.12.v20130726
    10818 [Thread-2] INFO org.eclipse.jetty.plus.webapp.PlusConfiguration - No Transaction manager found - if your webapp requires one, please configure one.
    12226 [Thread-2] INFO org.eclipse.jetty.webapp.StandardDescriptorProcessor - NO JSP Support for /, did not find org.apache.jasper.servlet.JspServlet
    12243 [Thread-2] INFO / - No Spring WebApplicationInitializer types detected on classpath
    12344 [Thread-2] INFO / - Initializing Spring root WebApplicationContext
    Initializing BerkeleyDB Match Database (creating indexes): Please wait...
    Initializing BerkeleyDB MD5 Database (creating indexes): Please wait...
    1049793 [Thread-2] INFO / - Initializing Spring FrameworkServlet 'mvc'
    Initializing BerkeleyDB Match Database (creating indexes): Please wait...
    Initializing BerkeleyDB MD5 Database (creating indexes): Please wait...
    1050000 [Thread-2] INFO org.eclipse.jetty.server.AbstractConnector - Started @0.0.0.0:8080

Note a "Address already in use" error would indicate that the lookup
service (or another existing service) appears to be already running on
that machine and port. Either stop the existing service, or configure
the lookup service to use a different port using the --port option.

Once successfully started the service will wait, ready to receive any
requests that are passed it's way. It will continue listening for
requests until the service is stopped. To confirm all is running
correctly you can now test the service.

Testing the service
-------------------

To test the service:

::

    # Assuming the lookup service has been started on the same machine and you are using
    # the default port of 8080 then...

    # in a web browser:
    http://localhost:8080/version
    http://localhost:8080/matches?md5=2E38C8D754C63117A4FA5F5E44F2194E

    # or using curl on the command line:
    curl http://localhost:8080/version
    curl http://localhost:8080/matches?md5=2E38C8D754C63117A4FA5F5E44F2194E

    # To access your lookup service from another machine replace "localhost" with
    # the fully qualified name of the machine where the lookup service is running.
    # The Linux command "uname -n" can be used to find the machine name.
    # Alternatively you could use the machines IP address instead of the hostname.

This should return an XML file containing match data (you may need to
"view source" on your web browser to see this properly).

If you leave it running then the lookup service is now ready to receive
any requests that may come it's way.

Configure InterProScan 5 to use your local lookup service
---------------------------------------------------------

To configure your local installation of InterProScan 5 to use your
lookup service, edit the ``interproscan.properties`` file and set the
property ``precalculated.match.lookup.service.url`` to point to your
service.

Replace **host** with the machine name and **port** with the port number
your server is running on:

::

    precalculated.match.lookup.service.url=http://host:port

    # Note: You can check your lookup service URL is accessible using curl on
    # the command line of the machine you will be running InterProScan from
    # For example, "curl http://host:port/" should return the expected HTML source

**For example**, if you are running the server on a machine named
**lookuphost** on **port 8080**, you should set the property as follows:

::

    precalculated.match.lookup.service.url=http://lookuphost:8080

**Or** if you are running the server on locally on **port 8080**, you
should set the property as follows:

::

    precalculated.match.lookup.service.url=http://localhost:8080

You can also substitute the server name with an IP address if necessary.

Please note that if you need to access the internet through a proxy
server then you will also need to update the following properties:

::

    precalculated.match.lookup.service.proxy.host=
    precalculated.match.lookup.service.proxy.port=3128
