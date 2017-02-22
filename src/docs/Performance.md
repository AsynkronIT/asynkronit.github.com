---
layout: docs.hbs
title: Performance
---

# Performance and Benchmarks

| Lib                | Remote PingPong   | Inproc PingPong   | SkyNet              |
| ------------------ | -----------------:| -----------------:| -------------------:|
| Proto.Actor C#     | ~2.5 mil msg/sec  | ~90 mil msg/sec   | ~0.9 sec            |
| Proto.Actor Go     | ~2.4 mil msg/sec  | ~120 mil msg/sec  | ~1.5 sec            |
| Proto.Actor Python | ?                 | ?                 | ?                   |
| Akka.NET           | ~50k msg/sec      | ~20 mil msg/sec   | ~15 sec             |

## Remote PingPong

This test uses two nodes and two actors, one on each node.
The test then pass 1 mil messages from node 1 to node 2 and back again.
There is no specific message size taken into account here, the message may be as small as
your framework supports.

## Inproc PingPong

This test is similar to the remote pingpong, the difference is that there is a single node and
there may be more than two actors, usually CPU-Count or CPU-Count * 2.
Messages may or may not be serialized. Both Proto.Actor and Akka.NET supports passing messages by reference
to optimize in-process performance.

## SkyNet

[https://github.com/atemerev/skynet](https://github.com/atemerev/skynet)

Creates an actor (goroutine, whatever), which spawns 10 new actors, each of them spawns 10 more actors, etc. until one million actors are created on the final level. Then, each of them returns back its ordinal number (from 0 to 999999), which are summed on the previous level and sent back upstream, until reaching the root actor. (The answer should be 499999500000).