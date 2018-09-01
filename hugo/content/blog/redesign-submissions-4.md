+++
title = "Redesign Submissions (Part 4)"
description = "Make submissions message flow easier to understand, scale and extend"
categories = ["general", "microservices"]
date = 2018-09-01T07:37:21+02:00
weight = 20
draft = false
+++

### Note on previous part

This is continuation of [Redesign Submissions (Part 3)](https://jalgoarena.github.io/blog/redesign-submissions-3/) post, where
you can find discussion on new design proposal on processing submissions. This post is iteration on described there
approach, which brings few improvements.

### Addressing open items

In the previous post we have mentioned few problems, which are tackled within this iteration.

#### Results Store

Firstly, there is new store - `Results Store`. Initially it was separated, and them moved again to single `Submissions Store`
as it required checking state when displaying submissions to users. But now, instead of keeping submission state we will go
to separate stores which has different use cases and schema.

* `Submissions Store` - the main purpose is to collect very initial data on submissions, and then report back to users
so they can monitor own submissions. This is like asynchronous response effect to user request, and that's the only purpose.
It's considered as secondary data - but still it's useful in investigation where submission is present and there is lack
of result.

* `Results Store` - main data set within `JAlgoArena` - that's the place where we store all submission data together with
assessment result and points gained for solving the problem. It's main source for calculating ranking, and it's displaying
all results on profile page of user.

#### Problem Ranking

As we display on user profile page results with points per solution - there is no need anymore to display it on UI on 
problem page - it was a bit of disconnected data which was necessary to understand best time bonus point - which is not
a case anymore. It simplifies architecture and UI a bit - making it more consistent and focused.

### Other changes

- layout improvements

### Diagram

![](https://raw.githubusercontent.com/jalgoarena/jalgoarena.github.io/master/images/data_flow_submission_2.3.png)

### Open questions

- should we keep synchronous calls to `Results Store` and `Submissions Store` - or would it be better to model it in
asynchronous request/reply way, using two related topics? That would give nice asynchronous flow running without 
blocking calls (besides of database call which anyway is blocking).

### Summary

It is one of first approaches to the design - which is subject to change. 

Check the next iteration on `Redesign Submissions (Part 5)`(coming soon) page.