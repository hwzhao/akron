# A crontab actor for Akka
[![Build Status](https://travis-ci.org/johanandren/akron.svg?branch=master)](https://travis-ci.org/johanandren/akron)

Currently for running on a single node actor system.

## Requirements
* Java 8
* Akka 2.4 or 2.5
* Scala 2.11 or 2.12

## How to use
Add to your build sbt:
```Scala
libraryDependencies += "com.markatta" %% "akron" % "1.2"
```

## Usage example
```Scala    
import com.markatta.akron._

val system = ActorSystem("system")

val crontab = system.actorOf(CronTab.props, "crontab")

val someOtherActor = system.actorOf(SomeOtherActor.props, "etc")

// send woo to someOtherActor once every minute
crontab ! CronTab.Schedule(someOtherActor, "woo", CronExpression("* * * * *"))

// there is also a type safe DSL for the expressions
import DSL._
crontab ! CronTab.Schedule(
  someOtherActor, 
  "wee", 
  CronExpression(20, *, (mon, tue, wed), (feb, oct), *))
  
```

Scheduling a job gives it an unique id which is sent back in a `CronTab.Scheduled` that can be used later
to unschedule that job. The crontab will watch the actor scheduled to receive messages for termination and
if terminated will remove all jobs sending messages to it.

`CronTab.Unschedule(id)` can be used to unschedule jobs.

`CronTab.GetListOfJobs` can be used to get a `CronTab.ListOfJobs` back with all current crontab jobs.


## Changelog

### 1.2

 * Artifacts for both Scala 2.11 and 2.12
 * Alpha/preview of persistent crontab (serialization format may change in the future) which will allow for running for example a cluster singleton crontab that is resilient.

### 1.1 

 * Order of hour and minute not according to "normal" cron expressions fixed
 
### 1.0
 
 * First release - all the things 
 

## License
Apache License, Version 2.0
