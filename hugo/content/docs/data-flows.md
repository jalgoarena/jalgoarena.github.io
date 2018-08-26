+++
title = "Data Flows"
description = "Describing data driven flows within JAlgoArena"
date = 2018-08-26T12:52:51+02:00
weight = 30
draft = false
bref = "It will explain all JAlgoArena specific data flows which models interaction of users with a platform"
toc = true
+++

<h3 class="section-head" id="h-intro"><a href="#h-intro">Intro</a></h3>

In JAlgoArena we distinct two areas of data flow, one which is fairly synchronous about creating user accounts, and then
using that information for accessing platform, authorizing users or just to enhance rankings.

Second, the core one is about processing user submissions, judging them and then storing results. Again, those results
together with user account data are used in ranking generation.

<h3 class="section-head" id="h-action-types"><a href="#h-action-types">Action types</a></h3>

There is few different type of activities which defines underlying technology and approach, below all used are listed and explained.

<table class="bordered striped">
    <tr>
        <th>Technology</th>
        <th>Activities</th>
        <th>Type</th>
    </tr>
    <tr>
        <td>REST API</td>
        <td>get, post</td>
        <td>synchronous</td>
    </tr>
    <tr>
        <td>Messaging</td>
        <td>publish, consume</td>
        <td>asynchronous</td>
    </tr>
    <tr>
        <td>Persistence</td>
        <td>store</td>
        <td>synchronous</td>
    </tr>
</table>


<h3 class="section-head" id="h-user"><a href="#h-user">User Account</a></h3>

<table class="bordered striped">
    <tr>
        <th>Activity</th>
        <th>Message flow</th>
    </tr>
    <tr>
        <td>Sign up</td>
        <td>1 post:user 2 store:save,kind:user 3 publish:user</td>
    </tr>
    <tr>
        <td>Log in (credentials)</td>
        <td>4 post:credentials 5 store:load,kind:user</td>
    </tr>
    <tr>
        <td>Log in (token)</td>
        <td>6 get:user 5 store:load,kind:user</td>
    </tr>
    <tr>
        <td>Query Users</td>
        <td>7 get:users 8 store:list,kind:user</td>
    </tr>
</table>

<h3 class="section-head" id="h-user"><a href="#h-user">Submissions</a></h3>

<table class="bordered striped">
    <tr>
        <th>Activity</th>
        <th>Message flow</th>
    </tr>
    <tr>
        <td>Submit solution</td>
        <td>
            9 post:submission 10 publish:submission,state:new 11 consume:submission,state:new 
            12 store:save,kind:submission 13 publish:submission,state:saved 
            14 consume:submission,state:saved 15 publish:submission,state:result 
            16 consume:submission,state:result 12 store:save,kind:submission 17 publish:ranking
        </td>
    </tr>
    <tr>
        <td>Query ranking</td>
        <td>18 get:ranking, 19 store:load,kind:ranking</td>
    </tr>    
</table>