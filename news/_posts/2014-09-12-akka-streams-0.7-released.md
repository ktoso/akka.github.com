---
layout: news
title: Akka Streams 0.7 Released!
author: Konrad Malawski
short: Akka Streams 0.7 Released
---

*Dear hAkkers,*

today we are very excited to share with you another early preview milestone of Akka Streams and Akka HTTP. 
Version 0.6 was not widely announced because we were preparing some major work on the API and most of that is now finished. 

Behold, the new FlowGraph DSL (we have implemented Scala first, the equivalent Java API will follow shortly):

    // first define some pipeline pieces
    val f1 = FlowFrom[Input].map(_.toIntermediate)
    val f2 = FlowFrom[Intermediate].map(_.enrich)
    val f3 = FlowFrom[Enriched].filter(_.isImportant)
    val f4 = FlowFrom[Intermediate].mapFuture(_.enrichAsync)

    // then add input and output placeholders
    val in = SubscriberSource[Input]
    val out = PublisherSink[Enriched]

(There are many more sources and sinks available, also for strict collections, iterators, futures and promises, etc.)

![akka streams processing graph]({{ site.baseurl }}/resources/images/news/akka-streams-07-example.png)

Then in order to connect these Flows into a complex stream processing graph  

    /*
     * then plug them together:
     */
    val g = FlowGraph { implicit b ⇒
      val bcast = Broadcast[Intermediate]
      val merge = Merge[Enriched]
    
      in ~> f1 ~> bcast ~> f2 ~> merge ~> f3 ~> out
      bcast ~> f4 ~> merge
    }.run()

    // now get out the real ins and outs for this run
    val sub = in.subscriber(g)
    val pub = out.publisher(g)

The previous Flow/Duct DSL — which is still present in the scaladsl & javadsl packages and will be removed in the next 
milestone – has now been unified in the scaladsl2 package and sliced differently: all operations that mediate between 
one input stream and one output stream (map, filter, etc.; also groupBy produces only one primary output stream) are 
defined on Flow, while all other “plumbing” operations are modeled as a graph that can capture any desired topology — 
cycles are an advanced feature that needs to be explicitly enabled though. FlowGraphs and Flows are immutable descriptions 
of a processing machine that can be set in motion by the run() method, which allows you to freely share references to these 
and write libraries that provide and consume them.

The second very exciting aspect of today’s milestone release is that is also incorporates the first part of the HTTP 
routing DSL that is being ported from Spray—as a first step we just incorporate the existing Scala part and in a second 
step we will add an equivalent Java API as well. To those of you who have used Spray before it should look very familiar,
on the surface it will remain as source compatible as we can make it. Upon closer inspection you will find that the DSL 
is now safer than before, its result type is no longer Unit but RouteResult (which will result in better compiler errors 
for nonsensical route definitions) and the evaluation of all elements is now consistently “lazy”, meaning that the inner 
part is not executed surprisingly for non-matching requests (that used to happen in certain edge cases).

As usual please be aware that this is still a very early development preview, we will likely change the API over the 
coming weeks (we will certainly move scaladsl2 back as scaladsl for example) but at this point the seas are calming down
and the weather should be approaching smooth sailing conditions soon.

Happy hakking!

## Closed tickets

The complete list of closed tickets can be found in the [streams-0.7 github issues milestone](https://github.com/akka/akka/issues?q=is%3Aissue+milestone%3Astreams-0.7+is%3Aclosed).

Akka Streams 0.7 is released for Scala 2.10.4 and 2.11.2. 
This release is backwards binary compatible with version 2.3.5 which means that the new JARs are a drop-in replacement for the old one (but not the other way around) as long as your build does not enable the inliner (Scala-only restriction). Always make sure to use at least the latest version required by any of your project’s dependencies.

### Credits ###

    commits added removed
          2    29       7 Roland Kuhn
          1   104     104 Martynas Mickevicius
          1     1       1 Richard Summerhayes
          1   181       3 Patrik Nordwall
          1   214       0 kerr
          1    71       1 Brendan Lawlor

*Happy hAkking!*
