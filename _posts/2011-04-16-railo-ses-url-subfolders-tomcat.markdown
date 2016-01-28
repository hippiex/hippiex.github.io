---
layout: post
title:  "Railo SES URL subfolder with Tomcat"
date:   2011-04-16 13:00:00
categories: railo ses jetty cfml tomcat
author: "Jeff R."
summary: "Getting SES URLs to work with the Tomcat install."
published: true
---

This is a continuation of my last post. In my effort to not have a million domain names and hosts file edits; I setup my Tomcat server on `EC2` the same way as the `Jetty` server on my laptop :).  This is to get `SES` urls to work  on Tomcat in a subfolder. For `http://my.ec2-server.com/site1/index.cfm/fuseaction/my.page/test/1` to function properly we do something similar to Jetty. This time we edit the `/opt/railo/tomcat/conf/web.xml` file.  Search for `/index.cfm/*`.

{% highlight  xml %}
<servlet-mapping>
<servlet-name>CFMLServlet</servlet-name>
<url-pattern>/index.cfm/*</url-pattern>
</servlet-mapping>
<servlet-mapping>
<servlet-name>CFMLServlet</servlet-name> 
<url-pattern>/site1/index.cfm/*</url-pattern> 
</servlet-mapping>
{% endhighlight %}

The restart `Railo`.
{% highlight  text %}
/opt/railo/railo_ctl restart
{% endhighlight %}

And you are all set.
