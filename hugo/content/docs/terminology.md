+++
title = "Terminology"
description = "JAlgoArena ubiquitous language"
date = 2018-08-02T07:31:06+02:00
weight = 40
draft = false
bref = "This page aims to make clear the vocabulary used across implementation and documentation within JAlgoArena domain"
toc = true
+++

<h3 class="section-head" id="h-intro"><a href="#h-intro">Intro</a></h3>

You will be able to see here all meaningful words appearing commonly in JAlgoArena domain. 

This is divided to two parts, domain which is more about business logic, and technology which is more about tools and infrastructure common to JAlgoArena
architecture. In case anything important is missing in here, please create a [new issue](https://github.com/jalgoarena/JAlgoArena/issues/new).

<h3 class="section-head" id="h-domain"><a href="#h-domain">Domain</a></h3>

<table class="bordered striped">
    <tr>
        <th>Term</th>
        <th>Description</th>
    </tr>
    <tr>
        <td>Submission</td>
        <td>The basic domain entity within JAlgoArena. It holds all necessery information for every user problem solution together with results after judgement. 
            The possible states of your submission are: 
            <ul>
                <li><strong>WAITING</strong> - just after submitting but before judgement</li>
                <li><strong>ACCEPTED</strong> - once your submission is accepted</li>
                <li><strong>COMPILE_ERROR</strong> - your submission source code is not compiling</li>
                <li><strong>RUNTIME_ERROR</strong> - there was runtime exception during tests of your submission</li>
                <li><strong>TIME_LIMIT_EXCEEDED</strong> - time limit was exceeded during tests of your submission</li>
                <li><strong>MEMORY_LIMIT_EXCEEDED</strong> - time memory was exceeded during tests of your submission</li>
            </ul>
            You can check all your submissions from main screen, clicking on <strong><em>Submissions</em></strong> menu item. 
        </td>
    </tr>
    <tr>
        <td>User</td>
        <td>Entity representing JAlgoArena user. After creating new account, your date is represented and kept secure under <strong>User</strong> entity. It's important to note that passwords are kept encrypted.</td>
    </tr>
</table>

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