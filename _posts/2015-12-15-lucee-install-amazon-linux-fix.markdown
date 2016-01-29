---
layout: post
title:  "Lucee Fix for Apache mod_cfml on Amazon Linux "
date:   2015-12-27 11:35:00
categories: lucee install cfml amazon linux ec2
author: "Jeff R."
summary: "Fix for Apache not serving Lucee pages on Amazon Linux. "
published: true
---

After installing [Lucee 4.5.2.018](http://lucee.org) with the [Vivio Technologies](https://www.viviotech.net) installer on Amazon Linux with `Apache` it wasn't serving `cfml` pages. Since Amazon Linux is a bit different the installer doesn't install the connector correctly.

Here is my fix for the issue on the 64bit version of Amazon Linux after running the [lucee-4.5.2.018-pl0-linux-x64-installer.run](http://lucee.org/downloads.html) installer:

{% highlight  text %}
#copy the mod_cfml for Apache 2.2 64bit from the lucee directory

cp /opt/lucee/sys/mod_cfml/centos-httpd22-x64/mod_cfml.so /usr/lib64/httpd/modules/

chmod 755 mod_cfml.so 

# Get the Shared Key from  the /opt/lucee/tomcat/conf/server.xml file

<!-- visit modcfml.org for details on mod_cfml configuration options -->
<Valve className="mod_cfml.core"
    loggingEnabled="false"
    maxContexts="200"
    timeBetweenContexts="2000"
    scanClassPaths="false"
    sharedKey="SHARED KEY" />

# Fill in the SHARED KEY and add the Valve to : /etc/httpd/conf/httpd.conf

DirectoryIndex index.cfm index.cfml index.html index.html.var

# add this to the end of httpd.conf
                
LoadModule modcfml_module modules/mod_cfml.so
CFMLHandlers ".cfm .cfc .cfml"
ModCFML_SharedKey "SHARED KEY FROM ABOVE"
# Optional, all for logging and debugging:
# LogHeaders true
# LogHandlers true
# LogAliases true
# VDirHeader false
{% endhighlight %}

Hopefully this will help out till the installer is modified.

Links :

[mod_cfml](http://www.modcfml.org/index.cfm/install/web-server-components/apache-on-centos/)

[Tomcat Value](http://tomcat.apache.org/tomcat-5.5-doc/config/valve.html)

[Lucee Bug Report](https://github.com/utdream/CFML-Installers/issues/74 )