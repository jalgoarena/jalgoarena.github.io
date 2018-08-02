+++
title = "Domain"
description = "JAlgoArena ubiquitous language"
date = 2018-08-02T07:31:06+02:00
weight = 40
draft = false
bref = "This page aims to make clear the vocabulary used across implementation and documentation within JAlgoArena domain"
toc = true
+++

<h3 class="section-head" id="h-intro"><a href="#h-intro">Intro</a></h3>

You will be able to see here all meaningful words appearing commonly in JAlgoArena domain. By domain we understand vocabulary
used to describe business logic. 

In case anything important is missing in here, please create a [new issue](https://github.com/jalgoarena/JAlgoArena/issues/new).

<h3 class="section-head" id="h-domain"><a href="#h-domain">Domain</a></h3>

<table class="bordered striped">
    <tr>
        <th>Term</th>
        <th>Description</th>
    </tr>
    <tr>
        <td>Submission</td>
        <td>The basic domain entity within JAlgoArena. It holds all necessery information for every user problem solution together with results after judgement. 
            The possible states of your submission are: 
            <ul>
                <li><strong>WAITING</strong> - just after submitting but before judgement</li>
                <li><strong>ACCEPTED</strong> - once your submission is accepted</li>
                <li><strong>COMPILE_ERROR</strong> - your submission source code is not compiling</li>
                <li><strong>RUNTIME_ERROR</strong> - there was runtime exception during tests of your submission</li>
                <li><strong>TIME_LIMIT_EXCEEDED</strong> - time limit was exceeded during tests of your submission</li>
                <li><strong>MEMORY_LIMIT_EXCEEDED</strong> - time memory was exceeded during tests of your submission</li>
            </ul>
            You can check all your submissions from main screen, clicking on <strong><em>Submissions</em></strong> menu item. 
        </td>
    </tr>
    <tr>
        <td>User</td>
        <td>Entity representing JAlgoArena user. After creating new account, your date is represented and kept secure under <strong>User</strong> entity. It's important to note that passwords are kept encrypted.</td>
    </tr>
    <tr>
        <td>Problem</td>
        <td>
            <p>Entity representing problem definition which can be solved by <em>users</em> by passing new <em>submission</em>.</p>
            <p>Every problem contains id, name, description, difficulty level and definition of skeleton code plus test cases to check against incoming <em>submissions</em></p>
            <p>All problems are pre-defined by administrators</em>
        </td>
    </tr>
    <tr>
        <td>Points</td>
        <td>
            <p>Users gain points for solving <em>problems</em>.</p>
            <p>To read more about how points are gained and how score is calculated, check <a href="https://jalgoarena.github.io/docs/calculating-points/" target="_blank">page/a>.</p>
        </td>
    </tr>
</table>
