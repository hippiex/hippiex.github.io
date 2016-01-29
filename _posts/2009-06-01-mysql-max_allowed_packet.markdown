---
layout: post
title:  "MySQL Updating max_allowed_packet"
date:   2009-06-01 13:00:00
categories: mysql
author: "Jeff R."
summary: "Fixing the annoying max packet error without rebooting the server."
published: true
---

This is just short post on settings the `max_allowed_packet` in MySQL without rebooting it.  I always forget how to do it when I need to change it quickly for whatever reason.

{% highlight  js %}
set global max_allowed_packet = 32 * 1024 * 1024;
{% endhighlight %}

This will set it for `32 MEGS`.  Change the `32` to have it larger or smaller.

Since this resets when the service restarts below is a link to the command line and `my.cnf` configuration information.

[MySQL max_allowed_packet](http://dev.mysql.com/doc/refman/5.7/en/packet-too-large.html)
