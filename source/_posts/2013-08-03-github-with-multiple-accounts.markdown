---
layout: post
title: "GitHub with Multiple Accounts"
date: 2013-08-03 11:03
comments: true
categories: [git github ssh]
---
When you need to work with more than one account on GitHub you'll need to get a little creative. This situation usually arises when you want to keep your personal account separate from your work account. The process is fairly straightforward and involves exercising SSH.

So, given that your existing SSH keys are your personal ones we'll make the second set for work.

## Create a New SSH Key
Create a new SSH key to use for the work account

> * You can also use ```-t dsa```, if you do just replace all references to ```rsa``` with ```dsa```.
> * Be careful **NOT** to overwrite your existing SSH key. Give this new key a unique name, e.g. ```~/.ssh/id_rsa_WORK```.

```
$ ssh-keygen -t rsa -C "your-email-address"
```

Add a new SSH key entry to your GitHub account by copying and pasting the contents of ```~/.ssh/id_rsa_WORK.pub``` into the SSH key field of your account.

If you are running an authentication agent you will need to add this new SSH key to it.

```
$ ssh-add ~/.ssh/id_rsa_WORK
```

## Edit the SSH Config File
If you do not already have the file ```~/.ssh/config``` create it and add something similar to the following

```
# Personal
Host github.com
  HostName github.com
  User git
  IdentityFile ~/.ssh/id_rsa

# Work
Host github-WORK
  HostName github.com
  User git
  IdentityFile ~/.ssh/id_rsa_WORK
```

## Sample Usage
In a nutshell, use the ```Host``` entry in your SSH commands instead of the actual host and SSH will use the correct information. For example, the following will use the new identity key.

```
$ git clone git@github-WORK:<account>/<repo>.git
```

