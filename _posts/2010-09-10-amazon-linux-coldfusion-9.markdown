---
layout: post
title:  "Amazon Linux Coldfusion 9"
date:   2010-08-15 16:22:22
categories: amazon ec2 cfml coldfusion 
author: "Jeff R."
summary: "Coldfusion 9 install instructions for Amazon Linux on EC2."
published: true
---

For fun this weekend I was playing with the Amazon Linux on EC2  with `Coldfusion 9`.  Since Amazon Linux is based on `CENTOS` I figured it shouldn't be too hard to get it to work so why not...  I'll make the disclaimer now that CENTOS and Amazon Linux are not officially supported, but we have been running Coldfusion 7/8 on CENTOS for a couple years now and it seems to work great once you get the installer and Apache connector to work.
I used Basic 64-bit Amazon Linux AMI 1.0 on a Micro T1 instance for my test.  Setting up launching and connecting to the instance is a bit out of reach for this post, but I just used the AWS console and followed the instructions to create my key-pair to SSH in.

Once you have the instance launched and you are connected you use `yum` to install Apache:

{% highlight  text %}
yum install httpd 
yum install httpd-devel
yum install libstdc++.so.5
{% endhighlight %}

*The httpd-devl and libstdc++.so.5 are required by the Coldfusion installer*

Then Run:

{% highlight  text %}
chkconfig httpd on
{% endhighlight %}

*This makes Apache start when the Image starts or you reboot it if you are on an EBS instance*

Then you need to get the Linux 64bit Coldfusion 9 off the Adobe Site.  It's kinda silly you can't get to it with `curl`, `wget`, or `lynx`.  So I downloaded it and put it on a private S3 Bucket then used `curl` to download it to my new instance

Then I followed [Willem Redelijkheid's post on installing on CENTOS](http://www.redelijkheid.com/blog/2010/4/1/adobe-coldfusion-9-on-centos-54-x64.html) for directions on installing Coldfusion 9 and it worked like a charm.  Only one slight oddity when the Installer progress bar got to the end on the script it sat there for the longest time.  I hit enter and it displayed the Coldfusion install finished method.  Not sure why.

Hope this helps someone else.  