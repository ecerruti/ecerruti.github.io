---
layout: post
title: "SVN to Git Migration"
date: 2013-08-01 11:41
comments: true
categories: [git svn vcs mac]
---
This is the process to migrate an SVN repository to a Git repository. Adjust paths to your particular scenario.

Create a Git repository, but do not add anything to it. It must be bare. For example, if on GitHub do **NOT** add any ignore or readme files.

> It is **IMPORTANT** that you do **NOT** go through the steps to clone the newly created repository or add anything to it. We are going to import the migrated SVN repository into it.
 
Now we need the SVN authors in nice Git format - store for safekeeping as it will be used later

```
$ cd /path/to/svn/repo
$ svn log --quiet | grep -E "r[0-9]+ \| .+ \|" | awk '{print $3}' | sort | uniq > authors.txt
```

> To cover scenarios ambiguous scenarios add a 'no author' with an email of your choosing.

Somewhere safe make a temporary working area

```
$ mkdir temp && cd temp
```

Pre-prep the SVN repository (locally), this has some additional options as an example

```
$ cd /somewhere/safe/temp
$ git svn clone https://svn.example.com/svn/path/to/repo -A ~/authors.txt --no-metadata --stdlayout --no-minimize-url --ignore-paths="^branches" --tags="tags/releases" --tags="tags/ci-builds"
```

Create a bare Git repository

```
$ cd /somewhere/safe/temp
# if already in the temp dir you can just use 'baremig.git'
$ git init --bare /somewhere/safe/temp/baremig.git
$ cd temp/baremig.git
$ git symbolic-ref HEAD refs/heads/trunk
```

Push the pre-prep SVN repository to the newly created baremig.git

```
$ cd /somewhere/safe/temp/OHS
$ git remote add bare /somewhere/safe/temp/baremig.git
$ git config remote.bare.push 'refs/remotes/*:refs/heads/*'
$ git push bare
```

Rename 'trunk' to 'master'

```
$ cd /somewhere/safe/temp/baremig.git
$ git branch -m trunk master
```

Migrate the SVN tags

```
$ cd /path/to/temp/new-bare.git
$ git for-each-ref --format='%(refname)' refs/heads/tags | cut -d / -f 4 | while read ref; do git tag "$ref" "refs/heads/tags/$ref"; git branch -D "tags/$ref"; done
```

Push the bare repository

```
$ cd /somewhere/safe/temp/baremig.git
$ git remote add origin ssh://git@github.com/<name>/<repo>.git
$ git push -u origin --all
```

> If the SSH 'git remote add' command does not work, try this one

> ```$ git remote add origin git@github.com:<username>/<repo>.git```

Push the tags to

```
$ git push --tags
```

Now go check it out. Once verified you can delete the local temp directory you worked from.

## Tips, Tricks and Hints

### SVN Command Line Error
If using the latest Xcode (version 4.6.2) and command line tools (/usr/bin/svn) with a Subversion version of 1.6.18 and you receive errors trying to check out the SVN repository

* Go to [Homebrew](http://mxcl.github.io/homebrew/) and install Subversion - use this svn (/usr/local/svn) instead of /usr/bin/svn - that is make sure /usr/local/bin comes before /usr/bin in your PATH

### Git SVN Command Line Error
When running the 'git svn' command and it errors out immediately you probably have to update the Perl SVN::Core CPAN module

* It will create ~/.cpan as root which can then chwon -R once done (I think it's OK to remove it, though, but probably best to wait until everything is completed)

```
$ sudo cpan SVN::Core
```

