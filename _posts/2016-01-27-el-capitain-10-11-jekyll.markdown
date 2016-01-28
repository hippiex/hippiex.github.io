---
layout: post
title:  "Installing Jekyll on El Capitain 10.11"
date:   2016-01-27 11:35:00
categories: jekyll install OSX gem
---

Oddly one my [iMac][imac] I had no issues with installing [Jekyll][jekyll], but on my [Macbook Air][macbook-air] it would error trying to install to `/usr/bin/`.  I did update the `gem` installer which may have caused the issues.  So I am noting this here for my future reference and including the link to the solution.

Updated gems.

{% highlight  js %}
sudo gem update --system
{% endhighlight %}

Install Jekyll to /usr/local/bin

{% highlight  js %}
sudo gem install -n /usr/local/bin jekyll
{% endhighlight %}

There are also [Homebrew][homebrew] install instructions, but I really wanted to to work with my normal OS X install since I don't use Ruby for anything, but [Jekyll][jekyll] at the moment.


[Jekyll Install troubleshooting OSX 10.11](http://jekyllrb.com/docs/troubleshooting/#jekyll-amp-mac-os-x-1011)

[jekyll]:		http://jekyllrb.com
[homebrew]:		http://brew.sh
[macbook-air]:	http://www.apple.com/macbook-air/
[imac]:		http://www.apple.com/imac/


