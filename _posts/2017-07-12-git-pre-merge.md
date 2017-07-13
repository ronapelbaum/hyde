---
layout: post
title: Git - prepare for Merge
desc: My first Git post! 
date: 2017-07-12
author: ronapelbaum
comments: true
publish: true
tags: [git]
---
## Git
Yes, I use git.

And, I enjoy working with branches and pull requests.

But every now and then I encounter this:

![pr-merge-conflict]({{ site.url }}/assets/pr-merge-conflict.png){:style="border: 2px solid grey"}

When this happens, you need to merge master into your branch.

If the merge is simple, with minimal conflicts, it can be done in github's UI. 
But if not, you better do it on your dev environment.  

## Before `git-merge`
I found myself doing the same things:

- update feature branch
- switch to master
- update master
- switch back to feature branch

Only after I'm sure that my local feature and master branch are updated, I can `git merge`.

## Lazy is good!
I'm a lazy developer, and [it's a good thing](http://threevirtues.com/).

I want to add a git command to do all the sh** for me.


## Add your personal Git command

For that you'll need:

1. Write you command.
2. Place it in PATH
3. `chmod +x`

#### 1. git-premerge

Let's create a bash file named `git-premerge`:
```bash
#!/bin/bash

current_branch=$(git branch | grep \* | cut -d ' ' -f2)

git pull

git checkout master

git pull

git checkout $current_branch

```
The `git-` prefix in filename is important.

#### 2. Place in path

Place the file in a directory that is already in your `PATH`. 
I put it in `/usr/local/bin` on my Mac.

An alternate option will be to place it in a dedicated folder (i.e. `~/my-git`), and add this folder to bash path.
To so, add `PATH=$PATH:~/my-git` to `~/.bashrc` file.

#### 3. `chmod +x`

To enable execution of that file.

## Let's Merge!

Now, before each merge, I just run:
```
git premerge
```

Have fun!

---

## Bonus track!

After I merged a lot, I have to many useless local branches..

Meet `git-purge`:
```bash
#!/bin/bash
echo "purging merged branches..."

git branch --merged | egrep -v "(^\*|master|dev)" | xargs git branch -d
```

This script lists al local branches that were merged already,
 except of `master` or `dev`.
 
Then it gives them as a param to `git branch -d`, which stands for `delete`.

Now, on updated master, run:
```
git purge

```
