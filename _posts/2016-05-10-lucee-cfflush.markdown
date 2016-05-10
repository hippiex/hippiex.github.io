---
layout: post
title:  "Lucee CFFLUSH problem and solution"
date:   2016-03-29 13:00:00
categories: linux lucee cfml fusebox
author: "Jeff R."
summary: "Debugging an unusual CFFLUSH issue with Lucee and CFPROCESSINGDIRECTIVE"
published: true
---

Some more notes on switching over from Coldfusion to Lucee server.  I have a custom reporting application that is running on Fusebox that I couldn't get the [CFFLUSH][cfflush] code I had to keep the status of the report connection updating.  It allowed updates as the report generated and kept Internet Explorer connections from timing out. At first I thought it was a problem with Lucee on Apache, but the following code worked in a plain old cfml page without Fusebox.


{% highlight  xml %}

<cfflush interval="1" />
<p>
	This page is running!
</p>
LuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLucee
		
<cfset sleep(10000) />

<p>
	This page Finished!
<p>

{% endhighlight %}

( The multitude of "Lucee"s are to fill up the buffer to test the [CFFLUSH][cfflush] and the sleep() function is to simulate a long running process. There appears to be a minimum you can send back with a flush no matter what the interval is )

Since this works it's not a Lucee/Apache issue.  After reviewing the fusebox4.runtime.cfmx.cfm there is a [CFPROCESSINGDIRECTIVE][cfprocessingdirective] tag for suppressing white space wrapped around the parsed files included by the framework.  So the following code in Lucee doesn't [CFFLUSH][cfflush] as expected. It waits for the entire page to run then processes the [CFPROCESSINGDIRECTIVE][cfprocessingdirective] and sends back the data.


{% highlight  xml %}
<cfprocessingdirective suppresswhitespace="Yes">
<cfflush interval="1" />
<p>
	This page is running!
</p>
		LuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLuceeLucee
		
<cfset sleep(10000) />

<p>
	This page Finished!
<p>
</cfprocessingdirective>
{% endhighlight %}

The above code also in Adobe Coldfusion. So to fix it I just removed the [CFPROCESSINGDIRECTIVE][cfprocessingdirective] and everything works as I was expecting. Interestingly enough if you turn on supresswhitespace in the Luceee Administrator and remove the [CFPROCESSINGDIRECTIVE][cfprocessingdirective] then the [CFFLUSH][cfflush] still works.


[cfprocessingdirective]:	http://docs.lucee.org/reference/tags/processingdirective.html
[cfflush]: http://docs.lucee.org/reference/tags/flush.html
