---
layout: post
title: "Prevent Desktop from Reverting to Default Images after a Mac Restart"
date: 2013-08-01 09:21
comments: true
categories: [mac, system configuration]
---
If your Mac reverts your desktop images back to the defaults after a restart try the following

```
$ rm -rf ~/Library/Preferences/com.apple.desktop.plist
# This file may or maynot exist
$ rm -rf ~/Library/Preferences/com.apple.desktop.plist.lockfile
```
Reboot. The desktop images will be the default, so go ahead and reset them to what you want. Restart and they should remain.

