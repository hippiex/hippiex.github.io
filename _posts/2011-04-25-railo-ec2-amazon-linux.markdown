---
layout: post
title:  "Installing Railo on EC2 with Amazon Linux"
date:   2011-04-25 13:00:00
categories: railo amazon ec2 linuz cfml
---

After following the Comments on Ben's blog about EC2 and Railo I dedcided to type up my notes in case they might help anyone. This assumes you know how to setup an account and the firewall. There is a GUI on the amazon site to do all this and setup your security files to SSH. If not google and you can find videos.

* Setup an EC2 account and launch an Amazon Linux AMI instance with the security settings to have ports :20, 21, 22, 80, 3306,8888 open.
* Once it's launched ssh to your new instance as ec2-user
* sudo su - ( logs you in as root )

*Update AMAZON LINUX*

{% highlight  ansi %}
yum update
{% endhighlight %}

*Install MYSQL*

{% highlight  ansi %}
yum install mysql mysql-server
{% endhighlight %}

This installs mysql just follow the prompts.

{% highlight  ansi %}
chkconfig mysqld on
{% endhighlight %}

This sets mysql to run on the start of the instance. ( edit the server settings in /etc/my.cnf with nano )

{% highlight  ansi %}
service mysqld start
{% endhighlight %}

This starts MYSQL on your instance for the first time.

{% highlight  ansi %}
cd /usr/bin/
./mysql_secure_installation
{% endhighlight %}

This file lets you setup the mysql root password and disable other stuff. The default mysql root password is "blank" so you must run this script.
{% highlight  ansi %}
CREATE USER 'remote_user'@'localhost' IDENTIFIED BY 'password';
GRANT ALL PRIVILEGES ON *.* TO 'remote_user'@'localhost';
CREATE USER 'remote_user'@'%' IDENTIFIED BY 'password';
GRANT ALL PRIVILEGES ON *.* TO 'remote_user'@'%';
{% endhighlight %}

% lets your user login from anywhere usually this is a bad idea. Set it to your IP address will be connecting remotely from. Localhost setups up the user so your Railo DSN's can talk to mysql. Change 'password' to a secure password you want to use.

*Apache*

{% highlight  ansi %}
yum install httpd httpd-devel
chkconfig httpd on 
service httpd start
{% endhighlight %}

This is similar to how we did mysql. If you navigate to your instance `http://[yourinstancename].compute-1.amazonaws.com/` you should get the apache start screen.

*Railo*

{% highlight  ansi %}
cd /tmp
wget http://railo.viviotech.net/downloader.cfm/id/15/file/railo-3.2.1.000-pl0-linux-x64-installer.run
chmod 777 railo-3.2.1.000-pl0-linux-x64-installer.run
./railo-3.2.1.000-pl0-linux-x64-installer.run
{% endhighlight %}

This runs the installer you'll need the folders below. On the 64bit version the defaults work. Apache is 2.2 for the connector.
{% highlight  ansi %}
/etc/rc.d/init.d/httpd
/usr/lib64/httpd/modules
/etc/httpd/conf/httpd.conf
http://[yourinstancename].compute-1.amazonaws.com:8888
http://[yourinstancename].compute-1.amazonaws.com:8888/railo-context/admin/web.cfm
http://[yourinstancename].compute-1.amazonaws.com:8888/railo-context/admin/server.cfm
{% endhighlight %}

Railo will now be running you can get to it by your EC2 name and port 8888 check the server and web admin to make sure the passwords are setup.
Configuring a new site on APACHE and RAILO
The sites need to be setup in railo and apache we'll setup a test site so you can see what to do.
On you laptop edit your hosts file and add an entry for :

{% highlight  ansi %}
999.999.999.999 mytestdomain.com
999.999.999.999 www.mytestdomain.com
{% endhighlight %}
Substitue your EC2 ip for 999.999.999.999 

On your EC2 instance :

{% highlight  ansi %}
cd /var/www/
mkdir mytestdomain.com
cd mytestdomain.com
mkdir html
cd html
nano index.cfm
cd ..
cd ..
chmod -R 777 mytestdomain.com
Setup the folder for Apache and Railo. And add a default index.cfm page with hello world in it. This is where your code will go.
nano /opt/railo/tomcat/conf/server.xml
{% endhighlight %}

At the end of the file near the example add:
{% highlight  xml %}
<Host name="www.mytestdomain.com" appBase="webapps"
unpackWARs="true" autoDeploy="true"
xmlValidation="false" xmlNamespaceAware="false">
<Context path="" docBase="/var/www/mytestdomain.com/html/" />
</Host>
{% endhighlight %}

This will add the webroot to railo.

{% highlight  ansi %}
/opt/railo/railo_ctl restart
{% endhighlight %}

This will reboot railo to see the new web folder.

{% highlight  ansi %}
nano /etc/httpd/conf/httpd.conf
ctr-w : index.html ( to find the line ) and add index.cfm to the end of the DirectoryIndex index.html index.html.var index.cfm line.
service httpd restart
{% endhighlight %}

Navigate to www.mytestdomain.com and you should be running Railo. This is just a summary of my notes to get everything running. Obviously your milage will vary and you need to know about running Apache,MYSQL, and railo :). 

Good Luck.

*Notes and stuff*

{% highlight  ansi %}
nano /opt/railo/tomcat/bin/setenv.sh [to setup memory settings for railo]
/opt/railo/railo_ctl restart [restart railo]
{% endhighlight %}