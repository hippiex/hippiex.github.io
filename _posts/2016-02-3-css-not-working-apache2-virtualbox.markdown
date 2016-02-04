---
layout: post
title:  "Virtualbox shared folder CSS Files not Updating"
date:   2016-02-03 19:00:00
categories: notes linux virtualbox css apache2
author: "Jeff R."
summary: "Apache2 Corrupting CSS files on a Virtualbox Share with Sendfile enabled"
published: true
---

So I was editing a website on my laptop running Apache2 with Lucee in Virtualbox. For some reason my CSS changes were not updating when I saved them. If I removed all the lines except for the first couple them it worked. Adding more lines would eventually corrupt the file being sent to the browser and no changes were displayed. I found the answer on a [Stack Overflow post][apache-fix]. in the httpd.conf file for Apache has `EnableSendfile on` this seems to be incompatible with the virtualbox file system. Sos switching it to `EnaleSendfile off` will fix the problem. Below are the instructions for Centos.

{% highlight  xml %}
nano /etc/httpd/conf/httpd.conf
ctrl-w EnableSendfile # search
# change on to off
ctrl-x # exit
y # to confirm
systemlctl restart httpd

{% endhighlight %}

[apache-fix]: http://stackoverflow.com/questions/6298933/shared-folder-in-virtualbox-for-apache