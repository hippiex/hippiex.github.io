---
layout: post
title:  "Lucee Locking Down Admin and Allowing CFGRAPH"
date:   2016-12-07 13:00:00
categories: linux lucee cfml fusebox apache
author: "Jeff R."
summary: "Allowing CFGRAPH with a locked down Lucee Admin"
published: true
---

I was reading through the [Lucee Lockdown][lockdown] guide for Apache and Lucee.  I locked down the admin fine, but using the following location directive.

{% highlight  xml %}

<Location /lucee>
  Order Deny,Allow
  Deny from all
  Allow from 999.999.9999.999 888.888.888
</Location>

{% endhighlight %}

This allows the 999.999.999.999 ip address and ip adresses that start with 888.888.888 to access the admin fine. You can add any number or ip adn partial ip addresses to this line separated by a space. Unfortunately it breaks the graphs [CFCHART][cfchart] generates.  To fix this issue you need to add following location directive after the above location directive.


{% highlight  xml %}

<location /lucee/graph.cfm>
  Order Allow,Deny
  Allow from All
</location>

{% endhighlight %}

This enables the graphs and still locks down the admin and can be applied to anything in the folder.


[lockdown]: http://docs.lucee.org/guides/cookbooks/lockdown-guide.html
[cfchart]: http://docs.lucee.org/reference/tags/chart.html
