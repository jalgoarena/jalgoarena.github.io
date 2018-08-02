+++
title = "Deployment"
description = "Up and running in 10 minutes"
date = 2018-08-02T12:08:06+02:00
weight = 40
draft = false
toc = true
bref = "Deployment instructions."
+++

<h3 class="section-head" id="h-consul-nomad"><a href="#h-consul-nomad">Consul & Nomad</a></h3>

The basis of JAlgoArena infrastructure are [Nomad](https://www.nomadproject.io/) and [Consul](https://www.consul.io/). 

On all machines where you plan to deploy JAlgoArena you should setup cluster of [Nomad](https://www.nomadproject.io/) and [Consul](https://www.consul.io/)

* use [nomad_consul_server.sh](https://github.com/jalgoarena/JAlgoArena-Nomad/blob/master/nomad_consul/nomad_consul_server.sh) to run locally on your laptop - it's not recommended for production use case
* [Install Consul Cluster](https://www.consul.io/intro/getting-started/join.html) - check firstly above command to use KV insert of config which you can modify, and nomad-client.hcl in case of using `raw_exec` driver
* [Install Nomad Cluster](https://www.nomadproject.io/intro/getting-started/cluster.html)

<h3 class="section-head" id="h-raw-exec"><a href="#h-raw-exec">Raw Exec (non-docker)</a></h3>

1. Once you have Nomad and Consul up and running, you may start running JAlgoArena external services:
   * Clone JAlgoArena-Nomad locally `git clone https://github.com/jalgoarena/JAlgoArena-Nomad.git`
   * go to cloned repo into `raw_exec` dir, and run [./step1.sh](https://github.com/jalgoarena/JAlgoArena-Nomad/blob/master/raw_exec/step1.sh) - this will run all necessary tools
     * make sure that traefik, kafka, zookeeper, cockrouch are seen healthy in your Consul Web UI
1. Run JAlgoArena microservices:
   * run [./step2.sh](https://github.com/jalgoarena/JAlgoArena-Nomad/blob/master/raw_exec/step2.sh) - this will run all microservices
   * after verifying all Nomad jobs are running and healthy within Consul - open JAlgoArena Web UI
     * if you are on machine where it's hosted - http://localhost:3000

<h3 class="section-head" id="h-docker"><a href="#h-docker">Docker</a></h3>

1. Make sure Docker is available on your machines
1. Once you have Nomad and Consul up and running, you may start running JAlgoArena external services:
   * Clone JAlgoArena-Nomad locally `git clone https://github.com/jalgoarena/JAlgoArena-Nomad.git`
   * go to cloned repo into `docker` dir, and run [./step1.sh](https://github.com/jalgoarena/JAlgoArena-Nomad/blob/master/docker/step1.sh) - this will run all necessary tools
     * make sure that traefik, kafka, zookeeper, cockrouch are seen healthy in your Consul Web UI
1. Run JAlgoArena microservices:
   * run [./step2.sh](https://github.com/jalgoarena/JAlgoArena-Nomad/blob/master/docker/step2.sh) - this will run all microservices
   * after verifying all Nomad jobs are running and healthy within Consul - open JAlgoArena Web UI
     * if you are on machine where it's hosted - http://localhost:3000
