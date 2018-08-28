+++
title = "Redesign Submissions"
description = "How to make submissions message flow easier to understand, scale and extend"
categories = ["general", "microservices"]
date = 2018-08-28T09:07:09+02:00
weight = 20
draft = false
+++

### Intro

Submissions flow withing JAlgoArena was already re-designed few times, mostly
I was focusing on technology and improvements around chosen technology.

The major upgrade happen when flow has changed from synchronous highly coupled
to asynchronous based on apache kafka with decoupled parties of flow.

Now I would like to stop and rethink all of that, without any technology in a mind.
The approach I want to choose is strictly based on requirements, and then
modeling messages - the only way to communicate in a new submissions world.

Having messages will open discussion on what exactly microservices we need
and how to define them keeping previously posted definition of microservice.

### Submitting new solution

Okay, let's think about how user interaction looks like:

- display problem page where solution can be submitted
- submit user solution

We have two main actions to focus on, let's figure out what are required messages
so we can properly generate problem page and then send submission with all
necessary data - so it may be assessed and published as result.

#### Display problem page

There is bunch of stuff we need, but some of data we need is already there. Firstly,
we assume case where user is logged in - lack of signed user leaves page in readonly
mode bringing no action at all. This gives use `username` - which together with
user token will authorize user submission. 

Another part is a problemId - we need to identify which problem is the subject matter.
This is another easy bit - as within UI we can use `redux` router and the path parameter
from url `http://localhost:3000/problems/{problemId}`.

Ok, let's think about what do we miss? Firstly, we need all details about problem:

- problem details like title, description, time and memory limit, problem difficulty
- another thing we need to have is skeleton source code

Here is an interesting thing - we need problem enhanced with generated skeleton code - and
that's ideal candidate for composition.

We will have:

* `get-problem`

{{< highlight json >}}
{
    "problem": "get",
    "problemId": "fib"
}
{{< /highlight >}}

* `get-problemWithSkeletonCode`

{{< highlight json >}}
{
    "problemWithSkeletonCode": "get",
    "problemId": "fib"
}
{{< /highlight >}}

As you can see, from UI we only need to invoke `get-problemWithSkeletonCode` and then
just internally invoke `get-problem`.

TBC