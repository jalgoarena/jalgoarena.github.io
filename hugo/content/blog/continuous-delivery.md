+++
title = "Continuous Delivery"
description = "Automated delivery of new features"
categories = ["general"]
date = 2018-07-25T07:42:36+02:00
weight = 20
draft = false
toc = true
+++

### Key factor to JAlgoArena development – ubiquitous automation

Nowadays we software engineers are in common agreement - automation is key to success
an DevOps movement just proves that. The goal of developing JAlgoArena from the very 
beginning was to make as much of automated deployment as possible - and in this post
I want to explain how does it look like.

### Deployment flow

1. Initially, developer push his changes to GitHub
1. In next stage, GitHub notifies Travis CI about changes
1. Travis CI runs whole continuous integration flow, running compilation, tests and generating reports
   - coverage report is sent to Codecov
   - using Gradle script builds and publishes new docker image for new tags (releases)
   - zip package is saved to GitHub releases for new tags (releases)
1. Nomad jobs takes artifact and deploy/scale automatically JAlgoArena components
   - docker images for docker based environment
   - zip packages for non-docker based environment

### Continuous Delivery diagram

![Continuous Delivery](https://github.com/spolnik/JAlgoArena/raw/master/design/continuous_delivery.png)

### Possible improvements

For now there is still couple of manual steps, some of them are such by design, others where yet not replaced:

- version change - as we use semantic versioning it requires wise changes to version numbers which is manual process
- marking tags - again, only developers know now which version is acceptable for further releasing
- manual version changes within Nomad jobs
- installing consul and nomad clusters before running nomad jobs

First two could be changed by e.g. date driven versioning, which is fairly popular recently, and e.g. automated postfix which would disappear
with releasing (like -SNAPSHOT in java world) - that's still something under consideration.

In regards of Nomad, there is still possibility to automate Consul cluster deployment with Nomad - ongoing clarification with HashiCorp on how to do it.
