---
layout: post
title:  "Adding SSL to Apache on EC2 with Amazon Linux"
date:   2016-03-03 13:00:00
categories: ec2 linux apache 
author: "Jeff R."
summary: "Notes for creating SSL certificates and adding to Apache on EC2 with Amazon Linux"
published: true
---

These notes assume you have Apache installed and working on EC2 with Amazon Linux, but it's fairly similar for other versions of Linux.

### Install OpenSSL and the Apache Connector

{% highlight  text %}
// for Apache 2.2
yum install openssl mod_ssl
// for Apache 2.4
yum install openssl mod24_ssl
// restart Apache
service httpd restart
{% endhighlight %}


### Test SSL

{% highlight  text %}

https://yoursever.com/

{% endhighlight %}

This will bring up the default key that was create when you installed OpenSSL.

### Generate Key

{% highlight  text %}

cd  /etc/pki/tls/private
openssl genrsa -out domain-name.key 2048
chown root.root domain-name.key
chmod 600 domain-name.key

{% endhighlight %}

### Generate Request

{% highlight  text %}
mkdir ssl under /ec2-user/domain-name/ssl
cd /ec2-user/domain-name/ssl
sudo openssl req -new -key /etc/pki/tls/private/domain-name.key -out domain-name.pem

{% endhighlight %}

Once the request has been generated and sent to your certificate authority they will send you back two .crt files. One is the domain cert and one is the bundle cert.  You can rename them to domain-name.crt and domain-name-bundle.crt.

{% highlight  text %}
// put crt file on the server with correct permissions
cp domain-name.crt /etc/pki/tls/certs/domain-name.crt
chown root.root /etc/pki/tls/certs/domain-name.crt
chmod 600 /etc/pki/tls/certs/domain-name.crt
  
cp domain-name-bundle.crt /etc/pki/tls/certs/domain-name-bundle.crt
chown root.root /etc/pki/tls/certs/domain-name-bundle.crt
chmod 600 /etc/pki/tls/certs/domain-name-bundle.crt
  
{% endhighlight %}

It's important to change the permissions on the file for Apache and OpenSSL will not work.

### Configure Apache SSL

{% highlight  text %}
// backup the conf file
cp /etc/httpd/conf.d/ssl.conf /etc/httpd/conf.d/ssl.conf.bkp
// edit the file
nano /etc/httpd/conf.d/ssl.conf


// search for the .key file line below and change the localhost.key

SSLCertificateKeyFile /etc/pki/tls/private/domain-name.key

// search for the .crt file line below and change the localhost.crt

SSLCertificateFile /etc/pki/tls/certs/domain-name.crt

// search for the bundle.crt file line below and point to the new bundle.crt

SSLCACertificateFile /etc/pki/tls/certs/domain-name-bundle.crt

// restart Apache
service httpd restart
{% endhighlight %}

This allows one SSL Domain on the server. If you want to have more than one SSL domain on the server it's a bit more setup. I'll cover that in a different post.


