---
layout: post
title: "Git productivity tip"
date: 2015-08-24 21:48:32 +0100
comments: true
categories: [git, productivity, merge]
---

git provides an efficient shortcut to refer to a previous branch: "-". It can be used to quickly switch to a previous branch or to merge current branch.

<!--more-->

For example if you switched from "master" branch to "feature" branch and now you want to switch back, just type:

```
git checkout -
```

and git will switch back to the "master" branch.

This is also useful for merging. If you want to merge "feature" branch in to the "master"  you can just do the following:

```
# switch to from "feature" branch to "master" branch
git checkout master
# merge "feature" branch in "master"
git merge -
```

This can require even less typing if you use <a href="https://github.com/robbyrussell/oh-my-zsh">oh-my-zsh</a> extension for zsh shell:

```
gco master
gm -
```
