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

### Pattern Matching based routing

To enable composition of microservices, we need to have a way to easily reconfigure
message flow so new specialized version of microservice can be plugged before or after
existing service. 

It can be useful for doing other stuff like caching component put in front of existing
service to improve performance - in general we want to enable additive design.

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

Another bit we need is problem ranking, as we display it from problem page (every problem
owns own ranking) we need to have a way to query for it.

* `get-problemRanking`
{{< highlight json >}}
{
    "ranking": "get",
    "type": "problem",
    "problemId": "fib"
}
{{< /highlight >}}

The interesting thing is, we need now 2 calls to build all data necessary to display page.
Interestingly, there is a better way to improve the situation which will be only shortly mentioned in here,
it's [GraphQL](https://graphql.org/learn/) data query and manipulation language.

#### Submit new solution

Once user solved solution and clicked on submit button, he generates new message:

* `queue-submission`
{{< highlight json >}}
{
    "queue": "submission",
    "sourceCode": "public class ...",
    "username": "jacek",
    "problemId": "fib"
}
{{< /highlight >}}

There is now interesting thing to consider, firstly we want to save this submission
as quickly as possible, but as it is done in asynchronous mode we want to get back
to user with info he may use to check state of his submission:

* `queue-submission-response`
{{< highlight json >}}
{
    "submissionId": "ABCD-1234"
}
{{< /highlight >}}

As you see, in whole asynchronous flow for processing submission the first step is
synchronous to let user know about accepted submission and the id of it.

Now we may start our message to run whole submission asynchronous flow, and yes first
step would be to ask for authorizing solution (even before it's saved):

* `authorize-solution`
{{< highlight json >}}
{
    "authorize": "solution",
    "sourceCode": "public class ...",
    "username": "jacek",
    "problemId": "fib",
    "submissionId": "ABCD-1234",
    "token": "Bearer 1234...",
    "submissionTime": now()
}
{{< /highlight >}}

To make sure submission has been submitted by same user as marked, we need to
check token against username, and once done we may put all user data into message.

Now, we can request saving solution

* `save-submission`
{{< highlight json >}}
{
    "submission": "save",
    "sourceCode": "public class ...",
    "user": {...},
    "problemId": "fib",
    "submissionId": "ABCD-1234",
    "submissionTime": DateTime
}
{{< /highlight >}}

Ok, now it's the time to judge solution - although there is missing piece of data about problem,
we need full details to be able to judge solution:

* `enhance-submission-with-problem`
{{< highlight json >}}
{
    "problem": "enhanceSubmission",
    "sourceCode": "public class ...",
    "user": {...},
    "problemId": "fib",
    "submissionId": "ABCD-1234",
    "submissionTime": now()
}
{{< /highlight >}}

And now finally we can sent full submission information to judge:

* `judge-submission`
{{< highlight json >}}
{
    "judge": "submission",
    "sourceCode": "public class ...",
    "user": {...},
    "problem": {...},
    "submissionId": "ABCD-1234",
    "submissionTime": now()
}
{{< /highlight >}}

Once solution is assessed, we can publish result:

* `publish-result`
{{< highlight json >}}
{
    "result": "publish",
    "sourceCode": "public class ...",
    "user": {...},
    "problem": {...},
    "submissionId": "ABCD-1234",
    "statusCode": "COMPLETED",
    "submissionTime": now(),
    "elapsedTime": 0.02,
    "consumedMemory": 12.02,
    "errorMessage": null,
    "passedTestCases": 6,
    "failedTestCases": 0
}
{{< /highlight >}}

The last message can be used to:

- store results
- update submissions
- publish new submission result event

### Diagram

![](https://raw.githubusercontent.com/jalgoarena/jalgoarena.github.io/master/images/data_flow_submission_2.0.png)

### Summary

It is one of first approaches to the design - which is subject to change once 
proposed design is implemented and challenged. 