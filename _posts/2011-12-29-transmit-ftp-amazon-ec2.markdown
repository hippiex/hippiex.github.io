---
layout: post
title:  "Connecting to EC2 with Transmit for Mac"
date:   2011-12-29 09:05:00
categories: transmit internet amazon ec2 OSX
author: "Jeff R."
summary: "Instructions for using the PEM Key file to connect to EC2 instance with Transmit."
published: true
---

This example is for connecting `Amazon Linux EC2` from a `Mac` so your mileage may vary.  

Click the `[+]` to add a new site to [Transmit][transmit]:

<img src="/images/ec2-transmit/transmit-add.jpg" style="border:1px solid silver;" />

Then make sure to select `sFTP` as your protocol. Enter either your `instance name` given to you by [AWS][aws], [elastic ip][elastic-ip], or the domain used to access it.  The user will be `ec2-user` with no password.  During the `EC2` setup you should have saved your `PEM key file` to your local machine. This allows using `SSH` from the terminal to manage the instance.  I usually put them in `~/Documents/ec2-keys/[yourkey].pem`. Then go to the [Terminal][os-x-terminal] and change directory to the `~/Documents/ec2-keys/` folder in this case and run :

{% highlight  text %}
chmod 700 [yourkey].pem
ssh-add -K [yourkey].pem
{% endhighlight %}

This will permanently add the key to your profile and then `Transmit` should connect correctly. If you have a problem make sure you run the `chmod` command as the `PEM` file needs to have restricted permissions.

[elastic-ip]: 	http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/elastic-ip-addresses-eip.html
[transmit]: 	https://panic.com/transmit/	
[aws]: 			https://aws.amazon.com 
[os-x-terminal]: https://en.wikipedia.org/wiki/Terminal_(OS_X)