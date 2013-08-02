---
layout: post
title: "Memcached Debugging"
date: 2013-08-01 14:03
comments: true
categories: [code php memcached debug]
---
Here is a useful PHP tool for debugging Memcached. You will need this file - [memcache.php](https://slalom.atlassian.net/wiki/download/attachments/3047465/memcache.php?version=1&modificationDate=1369252658701&api=v2).

## Mac

Setup

> If you do not have Apache running see the post [Enable Apache on a Mac](/blog/2013/08/01/enable-apache-on-a-mac). Alternatively, try [MAMP](http://www.mamp.info/) - adjust paths as necessary.

```
$ cd ~/
$ mkdir Sites
# move memcache.php to ~/Sites/
$ cd /etc/apache2
$ sudo vi httpd.conf
```

Search for 'php' - this line specifically

```
#LoadModule php5_module libexec/apache2/libphp5.so
```

Uncomment it (remove the leading '#') and save file.

Put ```memcache.php``` in ```~/Sites/```. To hit it

```
http://localhost/~<username>/memcache.php
```

memcache.php has a username/password setup

```
username: memcache
password: password
```

## Windows

If on Windows and not running Apache take a look at [WAMP](http://www.wamp.info/). In either case drop the PHP file in the appropriate place.

## Telnet Debugging

For telnet to memcached

```
$ telnet localhost 11211
```

Quick overview of the most useful telnet commands

```
> stats #lists a bunch-o-stuff
> stats items #lists the slabs, example output. STAT items:3:number 1
#list the items in slab id 3 up to 100 items (doesn't seem to work if you leave off the last number, although it can be any number)
> stats cachedump 3 100
> version #version of memcached
```

There are a bunch of others

> Not mentioned is 'gets' which returns a CAS token, if there.

