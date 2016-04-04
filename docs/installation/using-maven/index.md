---
layout: docs
title: Building the Shindig web module using Maven
headline: Building the Shindig web module using Maven
description: Bundle, customize and run the Shindig web module on your favorite Java web app server
---


# Overview
{:class="page-header"}

If you prefer managing the installation of Shindig server on an existing application server, this guide
will take you through a creation of a skeleton project where you can use your custom configuration.
{:class="lead"}

## Prerequisites

[Apache Maven](https://maven.apache.org){:target="_blank"} needs to be downloaded and installed.  
Follow the detailed instructions from the project's website according to your OS.

<img class='img-responsive' src='https://maven.apache.org/images/maven-logo-black-on-white.png'>

> Apache Maven is a software project management and comprehension tool. Based on the concept of a project object model (POM), Maven can manage a project's build, reporting and documentation from a central piece of information.

## Create a project skeleton

> The sample code can be found at the [Github repository](https://github.com/OpenDashboards/od-shindig-docker/tree/master/maven){:target='_blank'}.

You can easily create a customized version of Shindig that meets your specific requirements, where you can adjust the default Shindig properties
and containers configuration.

Create the following folder structure:

<pre><code class="language-bash" data-lang="bash">
shindig/
├── src/main/resources
│   └── containers/
│       ├─ default/
│       │    └── container.js
│       └── shindig.properties
└── pom.xml
</code></pre>

### Edit the pom.xml file

Edit the Maven **pom.xml** configuration file and add the following:

<pre><code class='xml'>

&lt;project xmlns=&quot;http://maven.apache.org/POM/4.0.0&quot; xmlns:xsi=&quot;http://www.w3.org/2001/XMLSchema-instance&quot; xsi:schemaLocation=&quot;http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd&quot;&gt;
  &lt;modelVersion&gt;4.0.0&lt;/modelVersion&gt;

  &lt;groupId&gt;org.opendashboards.shindig&lt;/groupId&gt;
  &lt;version&gt;1.0.0-SNAPSHOT&lt;/version&gt;
  &lt;artifactId&gt;od-shindig-example&lt;/artifactId&gt;

  &lt;name&gt;OpenDashboards - Shindig - Example Maven artifact&lt;/name&gt;

  &lt;packaging&gt;war&lt;/packaging&gt;

  &lt;!-- ====================================================================== --&gt;
  &lt;!-- Properties --&gt;
  &lt;!-- ====================================================================== --&gt;
  &lt;properties&gt;
    &lt;shindig.server&gt;2.5.2&lt;/shindig.server&gt;
    &lt;plugin.jetty&gt;9.3.8.v20160314&lt;/plugin.jetty&gt;
  &lt;/properties&gt;

  &lt;!--====================================================== --&gt;
  &lt;!-- Dependencies --&gt;
  &lt;!--====================================================== --&gt;
  &lt;dependencies&gt;

    &lt;!-- Shindig server --&gt;
    &lt;dependency&gt;
      &lt;groupId&gt;org.apache.shindig&lt;/groupId&gt;
      &lt;artifactId&gt;shindig-server-dependencies&lt;/artifactId&gt;
      &lt;version&gt;${shindig.server}&lt;/version&gt;
      &lt;type&gt;pom&lt;/type&gt;
    &lt;/dependency&gt;
    &lt;dependency&gt;
      &lt;groupId&gt;org.apache.shindig&lt;/groupId&gt;
      &lt;artifactId&gt;shindig-server-resources&lt;/artifactId&gt;
      &lt;version&gt;${shindig.server}&lt;/version&gt;
      &lt;type&gt;war&lt;/type&gt;
    &lt;/dependency&gt;

  &lt;/dependencies&gt;

  &lt;!--====================================================== --&gt;
  &lt;!-- Build configuration --&gt;
  &lt;!--====================================================== --&gt;
  &lt;build&gt;
    &lt;plugins&gt;
      &lt;plugin&gt;
        &lt;groupId&gt;org.apache.maven.plugins&lt;/groupId&gt;
        &lt;artifactId&gt;maven-war-plugin&lt;/artifactId&gt;
      &lt;/plugin&gt;

      &lt;!-- Jetty plugin --&gt;
      &lt;plugin&gt;
        &lt;groupId&gt;org.eclipse.jetty&lt;/groupId&gt;
        &lt;artifactId&gt;jetty-maven-plugin&lt;/artifactId&gt;
        &lt;version&gt;${plugin.jetty}&lt;/version&gt;
      &lt;/plugin&gt;

    &lt;/plugins&gt;

    &lt;finalName&gt;ROOT&lt;/finalName&gt;

  &lt;/build&gt;

&lt;/project&gt;


</code></pre>

The project's configuration file uses the latest stable version of Shindig web artifacts and bundles them
into a **.war** package, ready to be deployed to an application server.  For convenience, the
[Maven Jetty plugin](http://www.eclipse.org/jetty/documentation/current/jetty-maven-plugin.html){:target='_blank'}
has been included in the POM and you can run the service by typing:

<pre><code class="language-bash" data-lang="bash">
$ mvn jetty:run-war
</code></pre>

The command will build and package the Shindig web application and run it locally through Jetty.  Open a browser window and point to the following URL where you should see the default sample gadget container page that is bundled
with the standard installation of Shindig.

> [http://localhost:8080/containers/commoncontainer](http://localhost:8080/containers/commoncontainer/){:target='_blank'}

![Sample common container](/assets/images/getting-started/common-container.png "Sample common container"){:class='img-responsive'}
