---
layout: post
title:  "Yahoo! Slurp why so greedy?"
date:   2010-10-08 13:00:00
categories: internet
author: "Jeff R."
summary: "Fix for Yahoo! Slurp and other crawlers hanging your server."
published: true
---

Yesterday our servers were being hammered from one IP address: 67.195.37.172 which appears to be the Yahoo indexer.  Then today I read a tweet from `Ben Nadel` he was having the same thing happen.  So I thought I would write a quick post in case others were having the same issue.

On the Yahoo! [SLURP][slurp] page they suggest adding the following information to your robots.txt file.

{% highlight  text %}
Crawl-delay:10
{% endhighlight %}

The number is the seconds of delay before it sends another request.
This seemed to stop the problem unless [SLURP][slurp] just gave up.
In doing more research there appears to also be an entry you can make, but nothing supports it right now.

{% highlight  text %}
Visit-time: 1800-2330
{% endhighlight %}

I added it just in case.

[slurp]:		https://help.yahoo.com/kb/search/slurp-crawling-page-sln22600.html