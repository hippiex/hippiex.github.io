---
layout: post
title:  "Lucee Fix on Amazon Linux not Starting on Boot "
date:   2016-1-15 11:35:00
categories: lucee install cfml amazon linux ec2
author: "Jeff R."
summary: "Fix for Lucee not starting on boot. "
published: true
---

After installing [Lucee 4.5.2.018](http://lucee.org) with the [Vivio Technologies](https://www.viviotech.net) installer on Amazon Linux with Apache it won't automatically start when rebooting the `EC2` instance. Even if you select it to start during the install process.

Edit the `/etc/rc.d/rc.local` file ( It loads after everything else on the machine ) and add the following to the end of the file:

{% highlight  text %}

/opt/lucee/lucee_ctl start

{% endhighlight %}

I used `rc.local` since I have other things in there for my instance's boot process. The installer should put it in  /etc/init.d/ folder so that you can use `chkconfig`.

[Check out August Kleimo's Post on installing Lucee if you want to do it that way.](http://www.augustkleimo.com/install-lucee-on-an-aws-linux-ec2-instance/)
