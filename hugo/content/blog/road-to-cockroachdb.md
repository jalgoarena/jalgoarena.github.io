+++
title = "Road to Cockroach DB"
description = "How to scale your persistence keeping microservices design"
categories = ["general", "db"]
date = 2018-08-07T06:26:36+02:00
weight = 20
draft = false
+++

### Databases in microservices world

Databases ... yes databases. In new microservice architecture world so much impact is put on serverless or function style
components, where stateless code wins. And obviously that's a great idea, when you have no state you have no need to synchronize
nor to lock. Scaling is the matter of replication and understanding of flow is obvious in a immutable world.

But still - can you imagine product bringing business value which does not handle state? Let's imagine identity management - 
this always will require to store some user info. Some may say but you know, there is google, github, and other OAuth which
handles that for you - sure but it simply means you are using external service to store the data - still you need somewhere
db to exist.

Anyway, let's go back to **JAlgoArena** world. In this post I would like you to show how I dealt through months with data
and how different problems moved me to different solutions.

### With monolith it's simple, is it?

TBC 