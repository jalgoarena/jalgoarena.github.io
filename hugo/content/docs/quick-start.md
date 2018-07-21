+++
title = "Quick Start"
description = "Up and running in 10 minutes"
weight = 10
draft = false
toc = true
bref = "As a complete and highly performant algorithmic contest platform, JAlgoArena is here to help you get the most out of your own hosted contest. JAlgoArena comes with predefined problems (100+) to solve with different difficulty levels - software engineers with different skillset will be able to tackle problems meeting their skills. Starting up with JAlgoArena is fast and easy. Here's how to set up JAlgoArena, and what basic prerequisites are required."
+++

<h3 class="section-head" id="h-basic-template"><a href="#h-basic-template">Deploy with Docker & Nomad</a></h3>

<p>To deploy JAlgoArena - you will need to spin up Nomad and Consul clusters plus install Docker Engine</p>

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

<h3 class="section-head" id="h-supported-browsers"><a href="#h-supported-browsers">Supported Browsers</a></h3>

<p>JAlgoArena supports the latest, stable releases of all major browsers:</p>
<ul>
    <li>Latest Chrome</li>
    <li>Latest Firefox</li>
    <li>Latest Safari</li>
    <li>Latest Opera</li>
    <li>Microsoft Edge</li>
    <li>Internet Explorer 11</li>
</ul>


<h3 class="section-head" id="h-development"><a href="#h-development">Solving a first problem</a></h3>

After you open JAlgoArena Web UI - you will see below page.

![](https://raw.githubusercontent.com/jalgoarena/JAlgoArena/master/design/ui/home.png)

Click on learn more, which will move you to problems page. In here you can see all available problems for solving, with difficulty level assigned.

![](https://raw.githubusercontent.com/jalgoarena/JAlgoArena/master/design/ui/problems.png)
