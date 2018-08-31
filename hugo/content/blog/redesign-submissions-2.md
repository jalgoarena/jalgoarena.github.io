+++
title = "Redesign Submissions (Part 2)"
description = "Make submissions message flow easier to understand, scale and extend"
categories = ["general", "microservices"]
date = 2018-08-30T08:46:21+02:00
weight = 20
draft = false
+++

### Note on previous part

This is continuation of [Redesign Submissions (Part 1)](https://jalgoarena.github.io/blog/redesign-submissions/) post, where
you can find discussion on new design proposal on processing submissions. This post is iteration on described there
approach, which brings few improvements

### Problems microservice interaction

Thinking about the nature of data we deal with within JAlgoArena - if we consider submissions this set is changing
very frequently, with any submission of user, producing large data set. On the other side, users still is the matter of
change but with much smaller frequency (and submissions grows is coupled to amount of users), resulting in small data set.
Finally, we have problems which are considered as smallest set of data, changing infrequently which can easily be cached
in memory (around 1.5 mb).

This means that previous architecture proposal and considerations were not effective, where every submission was enriched
by problems service for every single submission request. That's unnecessary stress on problems microservice which
makes submissions flow longer, when in reality any service which requires problems data can request it on demand and cache.

That's actually pretty close on how it is done in current submissions architecture, with one minor difference - just in case
of change there is no auto refresh happening for problems.

Actually we already have solution for such cases, and that's why `problem:changed` is introduced, to asynchronously
inform everyone interested that they need to refresh list of problems - keeping it up to date.

### Other changes

* Minor updates to diagram layout and message notation, now it's strict to `<domain>:<action>`
* `get:problemwithSkeletonCode` is changed to `problem:get` to make it unified with another request to `problems` microservice
  * the reason for that is simple - `problem-skeleton-code` microservice is just special composition over `problems` microservice
    with intention of keeping same interface, and being composable
    
### Diagram

![](https://raw.githubusercontent.com/jalgoarena/jalgoarena.github.io/master/images/data_flow_submission_2.1.png)

### Summary

It is one of first approaches to the design - which is subject to change. 

Check the next iteration on [Redesign Submissions (Part 3)](https://jalgoarena.github.io/blog/redesign-submissions-3/) page.