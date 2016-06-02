---
layout: docs
title: A first example gadget
headline: A first example gadget
description: Writing a basic gadget and running it through Shindig
---

# Overview
{:class="page-header"}
This section will help you on board with using Shindig for rendering gadgets.
{:class="lead"}

## Writing a basic gadget

We will start off by writing a very simple gadget definition that will simply render
the infamous **Hello world!** on the screen.  Each gadget has its own XML definition file
and should be accessible by the Shindig server in order to be processed.

<div class="bs-callout bs-callout-info">
    <h4>Heads up</a></h4>
    <p>All gadgets presented in the tutorials are hosted as part of this web site for your own convenience.</p>
    <p>
      More information on writing gadgets can be found at the official <a href='https://developers.google.com/gadgets/' target='_blank'>Gadgets API</a>
      space which is part of the <b>Google Developers Network</b>.
    </p>
</div>

<div class="highlight"><pre><code class="xml">
&lt;?xml version="1.0" encoding="UTF-8" ?&gt;
&lt;Module&gt;
  &lt;ModulePrefs title="hello world example" /&gt;
  &lt;Content type="html"&gt;
     &lt;![CDATA[
       Hello, world!
     ]]&gt;
  &lt;/Content&gt;
&lt;/Module&gt;
</code></pre></div>

[Get the source](/examples/gadgets/reference/hello-world.xml){:target='_blank'}

The gadget code should be pasted into a file that is accessible from the running instance
of the Shindig container.  For this reason, if you want to run the gadget from your local
computer, you need to serve the file from an HTTP (or web) server like [Nginx](https://nginx.org/){:target='_blank'}
or [Apache](https://httpd.apache.org/){:target='_blank'}.

For the sake of brevity you can try running this gadget directly from the [URL](/examples/gadgets/reference/hello-world.xml){:target='_blank'}
within the web site.  Just copy the URL and head to the [sandbox sample container](http://localhost:8080/containers/commoncontainer/){:target='_blank'}
provided by the Shindig server.

Enter the copied gadget URL into the text box and press the '*Add only*' button.  The gadget should be rendered on the
right side of the screen and you should see '**Hello world**'.

![Hello world gadget](/assets/images/examples/gadgets/hello-world.png "Hello world gadget"){:class='img-responsive'}

Congratulations!  You have successfully rendered your first gadget on your new Shindig container.
