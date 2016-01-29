---
layout: post
title:  "Installing Jekyll on El Capitain 10.11"
date:   2016-01-27 11:35:00
categories: jekyll install OSX gem
author: "Jeff R."
summary: "Notes for permission issue installing Jekyll on El Capitain."
published: true
---
I am currently moving my blog to Github and [Jekyll][jekyll]. On my [iMac][imac] there were no issues installing Jekyll, but on my [MacBook Air][macbook-air] it errored trying to install files in `/usr/bin/`. There was also a single threading warning on my iMac so I updated the `gem` installer first on my second install. This may have caused the MacBook Air issues.  So I am noting this here for my future reference and including a link to the solution at the bottom of this post.

Update `gem`:

{% highlight  text %}
sudo gem update --system
{% endhighlight %}

Install Jekyll to` /usr/local/bin`:

{% highlight  text %}
sudo gem install -n /usr/local/bin jekyll
{% endhighlight %}

There are also [Homebrew][homebrew] install instructions, but I really wanted this setup with a "normal"  `Mac` install since I don't use Ruby for anything except [Jekyll][jekyll] at the moment.


[Jekyll Install troubleshooting OSX 10.11](http://jekyllrb.com/docs/troubleshooting/#jekyll-amp-mac-os-x-1011)

[jekyll]:		http://jekyllrb.com
[homebrew]:		http://brew.sh
[macbook-air]:	http://www.apple.com/macbook-air/
[imac]:			http://www.apple.com/imac/


