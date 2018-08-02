+++
title = "Deployment"
description = "Up and running in 10 minutes"
date = 2018-08-02T12:08:06+02:00
weight = 40
draft = false
toc = true
bref = "Deployment instructions."
+++

<h3 class="section-head" id="h-basic-template"><a href="#h-basic-template">Deploy with Docker & Nomad</a></h3>

To deploy JAlgoArena - you will need to spin up Nomad and Consul clusters plus install Docker Engine.

1. Make sure Docker is available on your machines
1. Install Consul and Nomad clusters
   * use [step0.sh](https://github.com/jalgoarena/JAlgoArena-Nomad/blob/master/step0.sh) to run locally on your laptop - it's not recommended for production use case
   * [Install Consul Cluster](https://www.consul.io/intro/getting-started/join.html)
   * [Install Nomad Cluster](https://www.nomadproject.io/intro/getting-started/cluster.html)
1. Once you have Nomad and Consul up and running, you may start running JAlgoArena external services:
   * Clone JAlgoArena-Nomad locally `git clone https://github.com/jalgoarena/JAlgoArena-Nomad.git`
   * go to cloned repo, and run [./step1.sh](https://github.com/jalgoarena/JAlgoArena-Nomad/blob/master/step1.sh) - this will run all necessary tools
     * make sure that traefik, kafka, zookeeper, cockrouch are seen healthy in your Consul Web UI
1. Run JAlgoArena microservices:
   * go to cloned repo, and run [./step2.sh](https://github.com/jalgoarena/JAlgoArena-Nomad/blob/master/step2.sh) - this will run all microservices
   * after verifying all Nomad jobs are running and healthy within Consul - open JAlgoArena Web UI
     * if you are on machine where it's hosted - http://localhost:3000
