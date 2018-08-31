+++
title = "Redesign Submissions (Part 3)"
description = "Make submissions message flow easier to understand, scale and extend"
categories = ["general", "microservices"]
date = 2018-08-31T12:48:16+02:00
weight = 20
draft = false
+++

### Note on previous part

This is continuation of [Redesign Submissions (Part 2)](https://jalgoarena.github.io/blog/redesign-submissions-2/) post, where
you can find discussion on new design proposal on processing submissions. This post is iteration on described there
approach, which brings few improvements.

### Small business feature change, significant architecture meaning

There was one cool (my subjective opinion ...) feature, which I was really happy about which was introduced already some time
ago. The feature was about adding bonus point to fastest solution for a given problem. In the first place, it really looks
nice as the feature to have, and allows to distinct best solutions from the tree of other solutions.

But, there is many problems with this approach:

- users complained that assigning bonus point is unfair. And this actually was true, as most of problems can be solved
with well designed solutions in less than millisecond, the winning was the matter of microseconds! And you can imagine
that luck was crucial in winning this point (many factors counted besides of efficiency, like how busy server was at the
time of assessing solution).
- it required context of all users submissions to tell how many points user will get for given submission. It was problematic
because submissions final points calculation could happen only in aggregate step
- it made whole calculation of points per submission more complex which required good understanding of problem ranking
itself

Giving up on this feature had interesting impact:

- new points service is created, which enriches every successful submission with points user gained for solving it (it
is just based on particular submission, without penalty points)
- now we can send points per submission back to user with `submission:list` message response
- all of that makes `ranking` microservice smaller and focused just on presenting and building ranking without points
calculation

### Other changes

- layout improvements
- add missing `submissions:list` message from `ranking` microservice to `Submissions Store`
- change color for store components
- rename `problems` microservice to `Problems Store`

### Diagram

![](https://raw.githubusercontent.com/jalgoarena/jalgoarena.github.io/master/images/data_flow_submission_2.2.png)

### Open items

- as you see now, from `results` microservice we send `result:add` - which does not correspond well to `Submissions Store`,
possibly there is a need for separate store for results?
- I would put problem ranking under question, if it's needed at all on problem page. Considering we have changed the way
of how points are calculated, and user can see them on submissions page there is no more need for additional clarity 
on problem web page.

### Summary

It is one of first approaches to the design - which is subject to change. 

Check the next iteration on `Redesign Submissions (Part 4)`(coming soon) page.