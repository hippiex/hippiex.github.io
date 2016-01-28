---
layout: post
title:  "Connecting to EC2 with Transmit for Mac"
date:   2011-12-29 09:05:00
categories: transmit ftp amazon ec2 linux
author: "Jeff R."
summary: "Instructions for using the PEM Key file to connect to EC2 instance with Transmit."
published: true
---

This example is for `Amazon Linux EC2` so your mileage will vary.  Click the `[+]` to add a site to [Transmit](https://panic.com/transmit/):

<img src="/images/ec2-transmit/transmit-add.jpg" style="border:1px solid silver;" />

You need to select `sFTP` as your protocol, enter either your instance name given to you by AWS or the domain you use to access it.  The user will be `ec2-user` with no password.  You should have saved your PEM key file to your local system to you can `SSH` and manage your instance.  I put it in `~/Documents/ec2-keys/[yourkey].pem`. Then go to the command line and change directory to the `~/Documents/ec2-keys/` folder in this case and run :


{% highlight  text %}
chmod 700 [yourkey].pem
ssh-add -K [yourkey].pem
{% endhighlight %}

