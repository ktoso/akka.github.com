---
layout: news
title: Akka Streams & Http 1.0 Released!
author: Konrad 'ktoso' Malawski
short: Akka Streams & Http 1.0 Released
---

*Dear hakkers,*
we—the Akka committers—are very pleased to announce the final release of Akka Streams & HTTP 1.0. 
After countless hours and many months of work we now consider Streams & HTTP good enough for evaluation and production use, 
subject to the caveat on performance below. We will continue to improve the implementation as well as to add features over 
the coming months, which will be marked as 1.x releases—in particular concerning HTTPS support (exposing certificate 
information per request and allowing session renegotiation) and websocket client features—before we finally add these new 
modules to the 2.4 development branch. In the meantime both Streams and HTTP can be used with Akka 2.4 artifacts since these 
are binary backwards compatibility with Akka 2.3.


### A Note on Performance ###
Version 1.0 is fully functional but not yet optimized for performance. 
To make it very clear: Spray *currently* is a lot faster at serving HTTP responses than Akka HTTP is. 

We are aware of this and we know that a lot of you are waiting to use it in anger for high-performance applications, 
but we follow a “correctness first” approach. After 1.0 is released we will start working on performance benchmarking 
and optimization, the focus of the 1.1 release will be on closing the gap to Spray.

### What Changed since 1.0–RC4 ###

  - Plenty documentation improvements on [advanced stages](https://github.com/akka/akka/pull/17966), 
    [modularity](https://github.com/akka/akka/issues/17337) and the [javadsl for Http](https://github.com/akka/akka/pull/17965),
  - Improvements to [Http stability under high load](https://github.com/akka/akka/issues/17854),
  - The [streams cook-book translated to Java](https://github.com/akka/akka/issues/16787),
  - A number of new stream operators: [recover](https://github.com/akka/akka/pull/17998) and [generalized UnzipWith](https://github.com/akka/akka/pull/17998) 
    contributed by Alexander Golubev,
  - The [javadsl for Akka Http](https://github.com/akka/akka/pull/17988) is now nicer to use from Java 8 and when returning Futures,
  - also Akka Streams and Http should now be [properly packaged for OSGi](https://github.com/akka/akka/pull/17979), thanks Rafał Krzewski!
  
The complete list of closed tickets can be found in the 1.0 [streams](https://github.com/akka/akka/issues?q=milestone%3Astreams-1.0)
and [http](https://github.com/akka/akka/issues?q=milestone%3Ahttp-1.0) milestones on github.

#### Credits ####

Since the RC4 release:

  - 32 tickets closed
  - 1558 files changed, 125283 insertions(+), 40765 deletions(-),
  - ... and a total of 9 contributors!

Statistics:

    commits   added removed
         26    2342     335 Johannes Rudolph
         11   10112      97 Endre Sándor Varga
          9     757     173 Martynas Mickevičius
          8    2821     487 Konrad Malawski
          3      28      49 2beaucoup
          3     701     636 Viktor Klang
          2      43       7 Rafał Krzewski
          2     801      42 Alexander Golubev
          1       8       8 Heiko Seeberger

*Happy hAkking!*