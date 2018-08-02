+++
title = "Technology"
description = "JAlgoArena external components"
date = 2018-08-02T07:35:06+02:00
weight = 40
draft = false
bref = "This page describes all external components used across JAlgoArena architecture"
toc = true
+++

<h3 class="section-head" id="h-intro"><a href="#h-intro">Intro</a></h3>

You will be able to see here all meaningful parts of JAlgoArena architecture which are not JAlgoArena microservices. 

In case anything important is missing in here, please create a [new issue](https://github.com/jalgoarena/JAlgoArena/issues/new).

<h3 class="section-head" id="h-technology"><a href="#h-technology">Technology</a></h3>

<table class="bordered striped">
    <tr>
        <th>Term</th>
        <th>Description</th>
    </tr>
    <tr>
        <td>Traefik</td>
        <td>API Edge service which exposes REST and WebSocket API doing necessary path rewrites and load balancing. See more details on <a href="https://traefik.io/" target="_blank">Traefik home page</a></td>
    </tr>
    <tr>
        <td>Apache Kafka</td>
        <td>
            <p>Stream processing component which controls flow for serving submissions.</p> 
            <p>Initially when users submits new solution, it goes to <em>Apache Kafka</em> from where it's consumed by <em>Submissions microservice</em> for futher processing. 
            All microservices which takes part in submission process communicate through <em>Apache Kafka</em> topics in asynchronous way.</p> 
            <p>See more details on <a href="https://kafka.apache.org/" target="_blank">Apache Kafka home page</a></p>
        </td>
    </tr>
</table>