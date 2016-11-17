title: how to sync forked repo on github
date: 2016-11-16 22:57:14
categories:
- programming
tags: git
---

after fork a project on github, you may also need to sync your repo with the original repo you forked from, how to do that?

1. add upstream
```
git remote add upstream git@github.intuit.com:xwei/iusclient.git
```
if you found you add the wrong upstream, you can remove it
```
git remote rm upstream
```
then add the right upstream
```
git remote add upstream git@github.intuit.com:xsbgoii/iusclient.git
```
2. then fetch upstream
```
git fetch upstream
```
3. merge the upstream with your forked repo
```
git merge upstream/develop
```
notice I forked the `develop` branch
