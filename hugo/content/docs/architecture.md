+++
title = "Architecture"
description = "High level view on JAlgoArena blocks"
date = 2018-07-25T07:31:44+02:00
weight = 20
draft = false
bref = "JAlgoArena is complex microservice based platform which focuses on high availability and performance"
toc = true
+++

<h3 class="section-head" id="h-intro"><a href="#h-intro">Intro</a></h3>

JAlgoArena conducts many parts, which can be divided to:

* Node.JS hosted Web UI
* Traefik as Edge Service
* Nomad as scheduler
* Consul for Service Discovery
* Apache Kafka used for messaging internally in the backend
* Elastic Stack for capturing distributed logs
* Cockroach DB for highly available data storage
* JAlgoArena microservices delivering core features

<h3 class="section-head" id="h-diagram"><a href="#h-diagram">Diagram</a></h3>

![Component Diagram](https://raw.githubusercontent.com/jalgoarena/JAlgoArena/master/design/component_diagram.png)

1. Publish submission to Kafka.
1. Save new submission (JAlgoArena-Submissions) & start judge process (JAlgoArena-Judge)
   1. Request submissions refresh via WebSocket subscriptions (JAlgoArena-Submissions)
1. Publish submission result
1. Store submission and ranking result (the second only if submission is accepted)
1. Request ranking & submissions refresh via WebSocket subscriptions

<h3 class="section-head" id="h-components"><a href="#h-components">Components</a></h3>

JAlgoArena microservices:

- [JAlgoArena UI](https://github.com/spolnik/JAlgoArena-UI) - host UI components, runs reverse proxy using Service Discovery for resolving edge service
- [JAlgoArena Auth Server](https://github.com/spolnik/JAlgoArena-Auth) - microservice responsible for authentication and storing users data
- [JAlgoArena Queue](https://github.com/spolnik/JAlgoArena-Queue) - microservice serving queue for incoming submissions, publishing them to Apache Kafka
- [JAlgoArena Submissions](https://github.com/spolnik/JAlgoArena-Submissions) - microservice storing all submissions and serving data based on collected data
- [JAlgoArena Judge](https://github.com/spolnik/JAlgoArena-Judge) - main microservice responsible for judging incoming submissions
- [JAlgoArena Ranking](https://github.com/spolnik/JAlgoArena-Ranking) - microservice focused on building ranking from successful submissions (ranking storage)
- [JAlgoArena Events](https://github.com/spolnik/JAlgoArena-Events) - microservice responsible for publishing events about submissions or ranking refreshes