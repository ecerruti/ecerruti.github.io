---
layout: post
title: "Enable Apache on a Mac"
date: 2013-08-01 09:09
comments: true
categories: [mac, apache, system configuration]
---
Create the Apache conf file for your user

```
$ sudo cat > /etc/apache2/users/`whoami`.conf
```

Add the following to that file (change ```<username>``` to your Mac's username)

```xml
<Directory "/Users/<username>/Sites/">
     Options Indexes MultiViews
     AllowOverride All
     Order allow,deny
     Allow from all
</Directory>
```

Complete the setup (this is where your sites will go)

```
$ mkdir ~/Sites
```

Start Apache

```
$ sudo /usr/sbin/apachectl start
```

This will also setup Apache to relaunch on system restarts.

To stop Apache, as well as disabling it for relaunch on system restarts,

```
$ sudo /usr/sbin/apachectl stop
```

To restart a running Apache

```
$ sudo /usr/sbin/apachectl restart
```

Access your sites as

```
http://localhost/~<username>/
```

