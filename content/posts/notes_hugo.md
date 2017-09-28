---
title: "My blog site setup tips"
type: post
date: 2017-09-21T16:10:27-04:00
draft: false
tags: [hugo, github, hucore, tech]
---

The first mistake you might have about [**Github Pages**](https://help.github.com/articles/user-organization-and-project-pages/) is about the subtle difference between *User & Org Pages* and *Project Page*. It takes me a while to realize that the first page can only point to the static content in master branch, which you don't have the possiblity of switching to gh-page branch or master/doc (you can do that with project repo though). So you only publish your generated static content in *User Page* as git submodule.

![master locked](/img/master-lock.png "Master branch is locked for User & Org Repo")

[Hugo deployment section](https://gohugo.io/hosting-and-deployment/hosting-on-github/) `Host GitHub User or Organization Pages` deliberates the steps and demonstrates how to set the git submodule.

Few themes also good for technical blogs: *After Dark*, *Hugo Nuo*, *Future Imperfect*, *Cactus*.
