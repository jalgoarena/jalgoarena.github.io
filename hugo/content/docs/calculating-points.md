+++
title = "Calculating Points"
description = "How my points in a ranking are calculated?"
date = 2018-07-24T19:22:55+02:00
weight = 20
draft = false
bref = "The article will help you to understand how your points are calculated"
toc = true
+++

<h3 class="section-head" id="h-problem-difficulty"><a href="#h-problem-difficulty">Problem Difficulty</a></h3>

Firstly, base amount of points depends on problem difficulty:

<table class="bordered striped">
    <tr>
        <th>Difficulty</th>
        <th>Base Points</th>
    </tr>
    <tr>
        <td>Easy</td>
        <td>10 points</td>
    </tr>
    <tr>
        <td>Medium</td>
        <td>30 points</td>
    </tr>
    <tr>
        <td>Hard</td>
        <td>50 points</td>
    </tr>
</table>
  
<h3 class="section-head" id="h-time-penalty"><a href="#h-time-penalty">Time Penalty</a></h3>

Secondly, amount of points which depends on time penalty:

<table class="bordered striped">
    <tr>
        <th>Elapsed Time / Time Limit</th>
        <th>Points Factor</th>
    </tr>
    <tr>
        <td>&gt;= 500</td>
        <td>1.0</td>
    </tr>
    <tr>
        <td>&gt;= 100</td>
        <td>3.0</td>
    </tr>
    <tr>
        <td>&gt;= 10</td>
        <td>5.0</td>
    </tr>
    <tr>
        <td>&gt;= 1</td>
        <td>8.0</td>
    </tr>
    <tr>
        <td>&lt; 1</td>
        <td>10.0</td>
    </tr>
</table>
  
> E.g. if time limit is 1 second, and you run your code in less than 1 millisecond 1 / 1 < 1 -> 10.0 points
  
<h3 class="section-head" id="h-other-rules"><a href="#h-other-rules">Other Rules</a></h3>

* In addition, problem solution with best time from all submissions (independently from language) gets 1 bonus point
* Every additional submission you get penalty:
  * e.g. if you submitted 2 times problem solution, 1 point will be taken from your final result
  * penalty is calculated till minimal amount of points for passed submission which is 1 point
* Only best of your submissions per problem is considered within your score

> Note: you can always verify points of your submission within problem ranking, which can be accessed on problem page