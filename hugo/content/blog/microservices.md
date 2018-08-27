+++
title = "Microservices"
description = "How to design microservices right"
categories = ["architecture"]
date = 2018-08-27T10:02:19+02:00
weight = 20
toc = true
draft = false
+++

### Intro

Nowadays every one is speaking about microservices and you can get an impression they are everywhere. 
There is obviously reason for that, as they are solving one of significant problems we faced with
monolithic applications - dealing with complexity and possibility to scale our software to meet the needs.

Now, we need to know that microservices are not equal - I do not think there is already community approved
single definition of microservice - and still there is many misleading or just wrongs ones around.

Foremost I would like to avoid following concepts of distributed monolith - where instead of dealing with
single process you divide your app to parts which are still tightly coupled although they are distributed
in a network - where you may gain some benefits but usually it's just adding more complexity over already
complex monolith app.

To avoid misconception of what do I mean by microservice - I would like to write down definition which is
closest to my heart and the way I see them and understand.

> It does not mean I've achieved that with JAlgoArena architecture, as I'm constantly learning the definition
I will post is actually based on my recent learning and all experience I get till the time, and something
I want to aim in long term architecture design

### Microservice Definition

Microservices are independently deployable processes communicating asynchronously using lightweight
mechanism focused on specific business capabilities running in an automated but platform- and language-
independent environment.

Additionally, microservice is an independent software component that takes no more than one iteration to build
and deploy.

### Explanations

Here is my comment on above, as it's pretty wide definition and leaves some space for more clarity.

Independently deployable process is pretty clear - and if we group it together with building it in a iteration
time - it will quickly mean there will be tens/hundreds of them and we have to have automated deployment
process. In JAlgoArena I'm using Nomad jobs to specify deployment and allow Nomad scheduler to do
everything needed to deploy and scale the platform. (I plan to do add Kubernetes based deployment too, although
you need to remember that Kubernetes is containers focused when Nomad allows on running any kind
of resource, like jar on any executable).

Communicating asynchronously using lightweight mechanism - that's neat requirement and meeting it
can make our architecture additive - I can add new functionality without touching any part of existing
components. To achieve that proper lightweight mechanism shall be added - and that's a bit of challenge
if you remember that in our definition we require it should be platform- and language- independent
environment. There is few technologies/approaches which can help to deal with that, like gRPC, Apache Kafka, REST,
WebSockets, routing using pattern matching, etc. But are all of them asynchronous by design?
Definitely REST/HTTP will not make it easy to do asynchronous way, and the question on lightweight
adds to that even more - if something requires sever do we still name it lightweight? gRPC seems
closest to address that - but that's definitely an area where I need to gain more knowledge and experience.
(Someone may say AKKA with actors would make it working - but I'm not sure this solution can be named
lightweight?).

### Gaps

There is still lots of work to do within JAlgoArena to make existing microservices close to the proposed definition.

The biggest challenges are:
* microservices (or some of them) are still too big
* much of communication (queries) is synchronous
* I'm using REST everywhere, but for internal microservices communication gRPC as example can be better
solution to match the desired microservices definition (lightweight asynchronous communication, programming 
language agnostic).  
