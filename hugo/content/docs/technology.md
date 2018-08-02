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
        <td>
            <p>API Edge service which exposes REST and WebSocket API doing necessary path rewrites and load balancing.</p>
            <p>See more details on <a href="https://traefik.io/" target="_blank">Traefik</a></p>
        </td>
    </tr>
    <tr>
        <td>Apache Kafka</td>
        <td>
            <p>Stream processing component which controls flow for serving submissions.</p> 
            <p>
                Initially when users submits new solution, it goes to <em>Apache Kafka</em> from where it's consumed by <em>Submissions microservice</em> for futher processing. 
                All microservices which takes part in submission process communicate through <em>Apache Kafka</em> topics in asynchronous way.
            </p> 
            <p>See more details on <a href="https://kafka.apache.org/" target="_blank">Apache Kafka/a></p>
        </td>
    </tr>
    <tr>
        <td>Consul</td>
        <td>
            <p>Consul cluster provides capability of service discovery and distributed configuration. <em>Traefik</em> is using consul to build all API rules based on which destination microservices can be reached.</p>
            <p>In production JAlgoArena should be using cluster build from 3 server agents for high availability</p>
            <p>See more details on <a href="https://consul.io/" target="_blank">Consul</a></p>
        </td>
    </tr>
    <tr>
        <td>Nomad</td>
        <td>
            <p>Nomad provides scheduling capability. Thanks to it JAlgoArena deployment is automated and using recent version of microservices and external components.</p>
            <p>
                Actual nomad jobs allows to use two drivers: 
                <ul>
                    <strong>docker</strong> - for which you have to have docker up and running on your machines, see <a href="https://www.nomadproject.io/docs/drivers/docker.html" target="_blank">official docs</a>
                    <strong>raw_docker</strong> - which requires to have Java 8 and Node.JS >= 6 available on your machines, see <a href="https://www.nomadproject.io/docs/drivers/raw_exec.html" target="_blank">official docs</a>
                </ul>
                All nomad job specs can be found in <a href="https://github.com/jalgoarena/JAlgoArena-Nomad" target="_blank">here</a>.
            </p>
            <p>See more details on <a href="https://nomadproject.io/" target="_blank">Traefik home page</a></p>
        </td>
    </tr>
</table>