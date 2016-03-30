---
layout: post
title:  "Setup of Lucee REST Web Services on Apache"
date:   2016-03-29 13:00:00
categories: linux lucee cfml
author: "Jeff R."
summary: "Getting REST Componets to work on Lucee / Apache / Linux"
published: true
---

I was pulling my hair out to get Lucee 4.5 to recognize REST CFCs on my Amazon EC2 server running Apache.  I am really writing this up as notes so I can find it later.

### Step 1

Go to the /etc/httpd/conf directory on your linux machine and edit the httpd.conf file.

{% highlight  text %}
nano httpd.conf

ctrl-w /rest

remove the # from the begining of  

ProxyPassMatch ^/rest/(.*)$ http://127.0.0.1:8888/rest/$1 

ctrl-x

save changes? Y

service httpd restart

{% endhighlight %}

### Step 2

Create a folder in your Lucee root for you test.

{% highlight  xml %}

mkdir /var/www/html/api/
cd /var/www/html/api/

nano hello.cfc

<cfcomponent rest="true" restpath="/hello"> 
    <cffunction name="myHello" access="remote" returnType="String" httpMethod="get" restPath="/world">
        <cfset res="Hello World!">
        <cfreturn res>
    </cffunction>
</cfcomponent>

{% endhighlight %}

### Step 3

Login to the Lucce admin and click on the Rest option on the side bar.

{% highlight  text %}
For now enable the List services.

Create a new mapping for /test /var/www/html/api/
{% endhighlight %}

<img src="/images/lucee-rest/rest-admin.png" style="" alt="Lucee Rest Admin Setting" />

### Step 4

{% highlight  text %}

Open a web browser and got to : http://yourserver.com/rest/ and hit enter.

You should see :

Available sevice mappings are:
*/rest/test

Then enter http://yourserver.com/rest/test/hello/world

You Should see:

"Hello World!"

{% endhighlight %}

