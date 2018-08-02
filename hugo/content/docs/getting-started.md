+++
title = "Getting Started"
description = "Solve your first problem"
weight = 10
draft = false
toc = true
bref = "Guidance on JAlgoArena UI screens."
+++


<h3 class="section-head" id="h-welcome"><a href="#h-welcome">Welcome</a></h3>

After you open JAlgoArena Web UI - you will see below page. You will have access to menu moving you to other screens.

> Note: the right up corner green bulb indicates that you are connected with WebSocket service. This service will bring you live updates to rankings and submissions.

![](https://raw.githubusercontent.com/jalgoarena/jalgoarena.github.io/master/images/welcome.png)

<h3 class="section-head" id="h-problems"><a href="#h-problems">Problems</a></h3>

Problems screen give you access to full list of problems. There is few things which are worth to know:

* you can see number of problems available - after clicking on it you will see maximum amount of possible to gain based on all problems
* all problems which you successfully solved will be marked green tick
* all problems which you submitted but did not solve will be marked by red cross
* you can filter problems which you did not solve yet
* you can filter problems based on difficulty
 

![](https://raw.githubusercontent.com/jalgoarena/jalgoarena.github.io/master/images/problems.png)

<h3 class="section-head" id="h-problem"><a href="#h-problem">Problem</a></h3>

Let's click now on one of the problems, famous fibonacci. Again, there is few things worth to know:

* problem which you successfully solved will be marked green tick
* problem which you submitted but did not solve will be marked by red cross
* you can see max amount of points to gain based on problem difficulty - and after clicking how in general different difficulty levels are scored
* at any point of time you may refresh this screen - you will lose your source code
* code editor is highlighting `Java` source code
* below source code editor you can see time and memory limits you cannot exceed
* after running your submission together with source code is saved and you can check it on <em>submissions</em> page

![](https://raw.githubusercontent.com/jalgoarena/jalgoarena.github.io/master/images/problem.png)

Additionally you can open problem ranking, where all problem accepted solutions are ranked against each other:

![](https://raw.githubusercontent.com/jalgoarena/jalgoarena.github.io/master/images/problem_ranking.png)

<h3 class="section-head" id="h-submissions"><a href="#h-submissions">Submissions</a></h3>

TBC

![](https://raw.githubusercontent.com/jalgoarena/jalgoarena.github.io/master/images/submissions.png)

... whoops! Something went wrong - let's check what ...

![](https://raw.githubusercontent.com/jalgoarena/JAlgoArena/master/design/ui/compile_error.png)

Yep right 😊 Let's check again source code we have submitted ...

![](https://raw.githubusercontent.com/jalgoarena/JAlgoArena/master/design/ui/source_code.png)

Seems obvious now 😊 As we run empty method which expected to return some value ... you see now. Ok, let's check ranking!

![](https://raw.githubusercontent.com/jalgoarena/JAlgoArena/master/design/ui/ranking.png)

Seems Kraków is leading competition 😊

Ok, after some effort and few days spent, let's check how my profile looks like!

![](https://raw.githubusercontent.com/jalgoarena/JAlgoArena/master/design/ui/profile_1.png)

... and how about my submissions ...

![](https://raw.githubusercontent.com/jalgoarena/JAlgoArena/master/design/ui/profile_2.png)

That's it for now 🐣!