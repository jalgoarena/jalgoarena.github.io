+++
title = "Road to Cockroach DB"
description = "How to scale your persistence keeping microservices design"
categories = ["general", "db"]
date = 2018-08-07T06:26:36+02:00
weight = 20
draft = false
+++

### Intro

To fully understand all words used across article - please refer to [domain page](https://jalgoarena.github.io/docs/domain/).

### Databases in microservices world

Databases ... yes databases. In new microservice architecture world so much impact is put on serverless or function style
components, where stateless code wins. And obviously that's a great idea, when you have no state you have no need to synchronize
nor to lock. Scaling is the matter of replication and understanding of flow is obvious in a immutable world.

But still - can you imagine product bringing business value which does not handle state? Let's imagine identity management - 
this always will require to store some user info. Some may say but you know, there is google, github, and other OAuth which
handles that for you - sure but it simply means you are using external service to store the data - still you need somewhere
db to exist.

Anyway, let's go back to **JAlgoArena** world. In this post I would like you to show how I dealt through months with data
and how different problems moved me to different solutions.

### On the beginning it's simple, is it?

When I started first prototype of **JAlgoArena** there was no separate storage tool, and design was very simple:

![Initial Design](https://raw.githubusercontent.com/jalgoarena/JAlgoArena/4e141f19b47ad7be100c96112059ebd88e7532ec/design/component_diagram.png)

State was there, but it was just hardcoded within Judge microservice. Not very useful for production use case, right?

That was the time when I started to think what should I choose, and how it should look like. By whole the time I had in my mind
the pattern that every microservice should have own database - and by the best efforts it should not be shared. 

Ok, I had already experience with **Mongo DB** and I loved how it managed state. Although I wanted more embedded approach,
when I can keep database really close to microservices. Lets see my first approach to real world storage handling: 

![First approach](https://raw.githubusercontent.com/jalgoarena/JAlgoArena/e13a8b73c0fa2bc60284e3b4deb76c453d0a2119/design/component_diagram.png)

Great - in here I was not yet great with all **DDD** approaches and domain separation, so please excuse me ;) My initial
understanding of database per microservices moved me to idea - so let's do a microservice with a responsibility for dealing
with a data (I know, I know ... it was not best idea)!

As my research went, I found [NeDB](https://github.com/louischatriot/nedb) as a great solution, I had my brand new NodeJS microservice
and could deal with data there. In reality - as you see on diagram I used json file for handling problems data - so **NeDB** wasn't my only choice.

The solution was pretty easy, you could save incoming json and then query for it (you can still check it on GitHub [page](https://github.com/spolnik/JAlgoArena-Data-Obsoleted)).
I was creating new file based storage for both contexts: users and submissions to keep them separate. By the way - it was first sign of
keeping two responsibilities within single service. Creating database was fairly easy, there was 
[simple code](https://github.com/spolnik/JAlgoArena-Data-Obsoleted/blob/master/server/newLocalDb.js) to create it:

```javascript
var Datastore = require('nedb');

module.exports = function (filename, logger) {
    var db = new Datastore({filename: filename, autoload: true});
    db.loadDatabase(function (err) {
        if (err) {
            logger.error(err);
        } else {
            logger.debug("DB loaded: " + filename);
        }
    });

    return db;
};
```

