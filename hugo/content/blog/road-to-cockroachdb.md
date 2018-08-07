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

{{< highlight javascript >}}
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
{{< /highlight >}}

Querying submissions looked like:

{{< highlight javascript >}}
submissionDb.find({userId: newSubmission.userId}, function (err, docs) {
    if (err) return next(err);
    return res.json(docs);
});
{{< /highlight >}}

All database operations you can find in [submissions.js](https://github.com/spolnik/JAlgoArena-Data-Obsoleted/blob/master/server/routes/submission.js),
and all users ones in [passport.js](https://github.com/spolnik/JAlgoArena-Data-Obsoleted/blob/master/server/config/passport.js).

By the way, another insight I would have on below was that database code was coupled with business logic - which in reality should be handled by some repositories.

As you see above, it was a bit better (finally we have real storage!) approach, but far from being ideal and we did not touch even scalability issues.

Oh ... one more point. At that time it was when I decided keeping problems within json was not a good idea. You know, we need **the** database ;)

![All within db](https://github.com/jalgoarena/JAlgoArena/raw/d94f858e259a5cfdc514a3b0ce0ca9f94996ee56/design/component_diagram.png)

Interestingly, at some of current versions I've reverted that approached and went back for json - as it's best serving this kind of data - and that's what I love
about microservices. Keeping them small and decoupled allows on quick and easy changes, even rewrite of whole service when needed :)

### Database per microservice

Ok, my whole soul, mind and body was fulfilled with the idea - keep database specific to microservice. And again, I wanted to use something which can sit together 
with microservice - use some kind of embedded database and make usage of it as simple as possible. Oh - and I wanted to switch to [Spring Boot](https://spring.io/projects/spring-boot)
which is de facto widely standard within Java ecosystem for creating microservices.

I looked for quite a while, compared different approaches and finally found - [Xodus](). However badly the name sounds, it's pretty decent database created by **JetBrains**.
**JetBrains Xodus** is a Java transactional schema-less embedded database - and they use it for own products e.g. __JetBrains YouTrack__. 
You may read more about it in [here](https://github.com/JetBrains/xodus/wiki). Just to be clear, it's not a kindergarten tool,
it is serious database capable to handle terabytes of data having transactions on board.

Let's see how it impacted the architecture:

![Xodus based solution](https://github.com/jalgoarena/JAlgoArena/raw/e9cc7ec915018ff154cb01f0246d5154ee77e89b/design/component_diagram.png)

Besides of the fact that whole architecture grew to use __Service Discovery__ approach, now you can see every microservice own
separate database. Let's see how some of the repository methods are implemented within **Kotlin** using Xodus:

{{< highlight kotlin >}}
fun findAll(): List<Problem> {
    return readonly {
        it.getAll(Constants.ENTITY_TYPE).map { Problem.from(it) }
    }
}
{{< /highlight >}}

There is bit of **Kotlin** know how to decrypt it - but it's enough to understand that __it__ is a thing passed from readonly
method, which looks like:

{{< highlight kotlin >}}
private fun <T> readonly(call: (PersistentStoreTransaction) -> T): T {
    return store.computeInReadonlyTransaction { call(it as PersistentStoreTransaction) }
}
{{< /highlight >}}

As you see, it's just wrapping the code into readonly transaction (btw, such structures is why I love **Kotlin**) and passing store
down to method, where we invoke getAll describing our entity name, and finally using **Kotlin** streams to map outgoing problems.

To make a full picture, let's see how write transaction looks like:
{{< highlight kotlin >}}
private fun <T> transactional(call: (PersistentStoreTransaction) -> T): T {
    return store.computeInTransaction { call(it as PersistentStoreTransaction) }
}
{{< /highlight >}}

And then how it is used to update existing items:

{{< highlight kotlin >}}
override fun addOrUpdate(problem: Problem): Problem {
    return transactional {
        val existingEntity = it.find(
                Constants.ENTITY_TYPE, Constants.problemId, problem.id
        ).firstOrNull()
        val entity = when (existingEntity) {
            null -> it.newEntity(Constants.ENTITY_TYPE)
            else -> existingEntity
        }
        updateEntity(entity, problem)
    }
}

private fun updateEntity(entity: Entity, problem: Problem): Problem {
    entity.apply {
        setProperty(Constants.problemId, problem.id)
        setProperty(Constants.problemTitle, problem.title)
        setProperty(Constants.problemDescription, problem.description)
        setProperty(Constants.problemLevel, problem.level)
        setProperty(Constants.problemTimeLimit, problem.timeLimit)
        setProperty(Constants.problemFunction, toJson(problem.func!!))
        setProperty(Constants.problemTestCases, toJson(problem.testCases!!))
    }
    return Problem.from(entity)
}
{{< /highlight >}}

Full source code is available in here - [XodusProblemRepository.kt](https://github.com/spolnik/JAlgoArena-Problems/blob/master/src/main/kotlin/com/jalgoarena/data/XodusProblemsRepository.kt).

This approach worked for quite a while - and I run few AlgoCup contest using it in production environment without any issues, or closely without any issues ;)

The biggest challenge was, that any single component which was using it was hard to scale horizontally. If I start replicating services, 
how do I synchronize those file based storages?

And there was actually partial rescue:

![Apache Kafka for rescue](https://github.com/jalgoarena/JAlgoArena/raw/900a3cb2df2eadfac11133a3932a3ecd0f9bc406/design/component_diagram.png)

That was huge change for a platform - making it reactive with all the writes (besides of creating accounts). That allowed
to keep data log with Kafka, and replicate as many instances of Ranking or Submissions services as we want. But ... there
is always some but ;)

Authentication was not coming through Apache Kafka. Till security is introduced within **JAlgoArena** on Apache Kafka level,
I cannot imagine keeping there user names and passwords and recover from it ;) The only choice is to still use synchronous
approach - so we cannot scale in here with **Xodus** database - we need scalable storage by itself and it's hard to achieve
with embedded storage.

### Scalability matters - Cockroach DB coming

TBC