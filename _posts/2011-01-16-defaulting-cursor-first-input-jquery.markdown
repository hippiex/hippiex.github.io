---
layout: post
title:  "Defaulting Cursor to First Input Field with JQuery"
date:   2011-01-16 13:00:00
categories: jquery javascript
author: "Jeff R."
summary: "Popping the cursor into the first input box on page load."
published: true
---

Here's a simple `javascript` example using `JQuery`to put the cursor in the first text box on a web page.  I added this to one of our sites so the cursor starts in the first text box on all the pages of our site.

{% highlight  html %}
<script type="text/javascript">
<!-- //
$document.ready(function()){
$("input[type='text']:first").select();
};}
// -->
</script>
{% endhighlight %}

It's using JQuery to find the `FIRST :  INPUT` tag with a `TYPE="TEXT"` on the page.  Then it `selects()` it to move the cursor there automatically for the user.