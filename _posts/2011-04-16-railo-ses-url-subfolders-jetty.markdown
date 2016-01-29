---
layout: post
title:  "Railo SES URL subfolder with Jetty"
date:   2011-04-16 13:00:00
categories: railo ses jetty cfml install
author: "Jeff R."
summary: "Getting SES URLs to work with the Jetty install."
published: true
---

I've been working my way through learning the ins and outs of `Railo` over the last few months.  So far I'm impressed, but still trying to figure out all the quirks.

Our sites use `Fusebox` and `SES` url's and for some reason they would work on my EC2 instances and not on my laptop.  If I searched for a solution most of the posts were old and pointed to  `.cfm/*` and `.cfc/*` needing to be added to the `/railo/etc/defaultweb.xml`.  It seems in the latest download these are already added.  So that didn't help me.  I finally figured it out though.  When I develop on my laptop I have all my webs in sub-folders under the main web root : `http://localhost:8888/cfbuilder/site1/index.cfm/fuseaction/my.page/test/1` for example. The mappings only seem to work if the site is in the root. 

So to get this to work you have to add: 

{% highlight  xml %}
<servlet-mapping>
<servlet-name>CFMLServlet</servlet-name>
<url-pattern>*.cfm</url-pattern>
<url-pattern>*.cfml</url-pattern>
<url-pattern>*.cfc</url-pattern>
<url-pattern>*.cfm/*</url-pattern>
<url-pattern>*.cfml/*</url-pattern>
<url-pattern>*.cfc/*</url-pattern>
<url-pattern>/*.cfm/*</url-pattern>
<url-pattern>/cfbuilder/site1/index.cfm/*</url-pattern>
</servlet-mapping>
{% endhighlight %}

It's kind of a pain to add each site manually to the file, but it works. So I can't complain too much.