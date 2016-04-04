---
layout: docs
title: Installing Shindig as a Docker container
headline: Installing Shindig as a Docker container
description: Download and install Shindig in your environment by following a few simple steps
---


# Overview
{:class="page-header"}

Shindig is bundled as a Docker image that can run on virtually any OS environment.
{:class="lead"}

## Prerequisites

The only requirement for running the service locally or on any type of server, is to download and install [Docker](http://docker.io){:target="_blank"}.

<img class='img-responsive' src='https://upload.wikimedia.org/wikipedia/commons/7/79/Docker_%28container_engine%29_logo.png'>

> Docker is a platform for developers and sysadmins to develop, ship, and run applications. Docker lets you quickly assemble applications from components and eliminates the friction that can come when shipping code. Docker lets you get your code tested and deployed into production as fast as possible.

### Download and install the Docker runtime

Download and install the Docker runtime for your type of OS by following the [instructions on the web site](https://docs.docker.com/installation/){:target="_blank"}.   

## Install the service

### Download and install the Shindig server image

After you have successfully installed the Docker Engine on the target machine, now it's time to download and run the container image that will be hosting the service.  Copy and paste the following command on a terminal:

<div class="highlight"><pre><code class="language-bash" data-lang="bash"><span class="gp">$ </span> docker pull opendashboards/od-shindig-docker:bootstrap</code></pre></div>

The command will download and install the latest image of the service on the target machine.  

<div class="bs-callout bs-callout-info" id="jquery-required">
    <h4 id="jquery-required">Heads up</a></h4>
    <p>Depending on your OS / environment, you might be required to have <b>administrative</b> rights in order to execute the above commands.</p>
    <p>Also the initial installation of Docker Engine and other images will take some time - depending also on your internet connection -
    so be patient.
    </p>
</div>

### Start a new Docker container

After the service has been successfully downloaded and installed, it's time to run it.  Copy the following command in the terminal to get it started.

<div class="highlight"><pre><code class="language-bash" data-lang="bash">
<span class="gp">$ </span> docker run -d -p 8080:8080 --name="shindig" opendashboards/od-shindig-docker:bootstrap
</code></pre></div>  

If the command executes successfully, you should see the ID of the container instance that was just launched by the Docker Engine on the terminal.

<div class="bs-callout bs-callout-info" id="jquery-required">
    <h4 id="jquery-required">Service logs</a></h4>
    <p>
    Since we launched the service as a background process, there will be no logs printed out in this terminal window.  In order to view any logging
    activity from the service, you should run the following command on either the same or a different terminal window:
    </p>

<div class="highlight"><pre><code class="language-bash" data-lang="bash">
<span class="gp">$ </span> docker logs -f shindig
</code></pre></div>

<p>
	The <b>-f</b> argument tells Docker to leave the log output stream running forever, i.e. as you will be using the service you will be able to
	see results in real time printed out in the console.
</p>

</div>

### Managing the container

Once the service container has been started, you can easily manage its state by executing the following Docker command in a terminal window:

<div class="highlight"><pre><code class="language-bash" data-lang="bash">
<span class="gp">$ </span> docker <b>start</b> | <b>stop</b> | <b>restart</b> shindig
</code></pre></div>

Using the appropriate Docker command, you can now start, stop or restart the service.

### Checking everything is in place

If everything went according to plan, it's time for a final check to see whether our service is up and running.  Open a browser window and point to the following URL

> [http://localhost:8080/containers/commoncontainer](http://localhost:8080/containers/commoncontainer/){:target='_blank'}

If everything went according to plan, you should see on your browser the default sample gadget container page that is bundled
with the standard installation of Shindig.

![Sample common container](/assets/images/getting-started/common-container.png "Sample common container"){:class='img-responsive'}


## Customizing the Docker image

> The sample code can be found at the [Github repository](https://github.com/OpenDashboards/od-shindig-docker/tree/master/customized){:target='_blank'}.

<pre><code class='docker'>

##################################################################
# OpenDashboards - Shindig - Docker - Bootstrap server
#
# Description
# -----------
# Creates a Docker image containing the latest stable version
# of the Apache Shindig web app.
#
# Author: Chris Spiliotopoulos (@chefArchitect)
##################################################################

# Extend the official Jetty image
FROM jetty:9.3.7

MAINTAINER Chris Spiliotopoulos (@chefArchitect)

# Download the latest stable version of the Apache Shindig web module from Maven Central
RUN wget http://central.maven.org/maven2/org/apache/shindig/shindig-server/2.5.2/shindig-server-2.5.2.war -O ROOT.war

# Unzip the .war package into the webapps Jetty directory
RUN unzip ROOT.war -d webapps/ROOT

#
# Set some environment variables
#

# The default Shindig host - localhost
ENV SHINDIG_HOST localhost

# The default Shindig port - 8080
ENV SHINDIG_PORT 8080

#
# Launch the Jetty engine
#

CMD java -Djava.io.tmpdir=/tmp/jetty -Dshindig.host=$SHINDIG_HOST -Dshindig.port=$SHINDIG_PORT -jar /usr/local/jetty/start.jar jetty.port=$SHINDIG_PORT


</code></pre>

The Shindig image is extending the latest version of the Jetty server image, downloads the latest
Shindig web artifact from the central Maven repository and runs the web app in a Jetty instance, exposing
port *8080*.

### Overriding the default Shindig properties

You can easily create a customized version of Shindig that meets your specific requirements, where you can adjust the default Shindig properties
and containers configuration.

Create the following folder structure:

<pre><code class="language-bash" data-lang="bash">
shindig/
├── containers/
│   └── default/
│       └── container.js
├── shindig.properties
└── Dockerfile
</code></pre>

**Dockerfile**

<pre><code class='docker'>

##################################################################
# OpenDashboards - Shindig - Docker - Customized server
#
# Description
# -----------
# Extends the basic Shindig image and adds custom configuration
#
# Author: Chris Spiliotopoulos (@chefArchitect)
##################################################################

# Extend the bootstrap Shindig image
FROM opendashboards/od-shindig-docker:bootstrap

MAINTAINER Chris Spiliotopoulos (@chefArchitect)

ADD shindig.properties webapps/ROOT/WEB-INF/classes/shindig.properties
ADD containers webapps/ROOT/WEB-INF/classes/containers

#
# Launch the Jetty engine
#

CMD java -Djava.io.tmpdir=/tmp/jetty -Dshindig.host=$SHINDIG_HOST -Dshindig.port=$SHINDIG_PORT -jar /usr/local/jetty/start.jar jetty.port=$SHINDIG_PORT

</code></pre>

#### Build the custom image

<pre><code class="language-bash" data-lang="bash">
$ docker build --tag="opendashboards/od-shindig-docker:customized" .
</code></pre>
