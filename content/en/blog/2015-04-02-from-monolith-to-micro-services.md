---
date: "2015-04-02"
summary: Micro-services are not (only) about technology
categories:
- developer
tags:
- micro-services
- scalability
- architecture
title: From Monolith to Micro-Services
---

source: [Micro-Services for Dysfunctional Teams](http://dejanglozic.com/2015/03/03/micro-services-for-dysfunctional-teams/)

> When I am asked to do an elevator pitch about advantages of micro-services, this list typically comes to mind:

> * Individually deployable pieces of running software each responsible for a small number of tasks
* Each micro-service can be implemented using a different stack
* Horizontal scalability decisions can be made at a micro-service level

> When you analyze this list, neither point is really making your system better from a purely technical point of view. In fact, a monolithic system is definitely easier to work with when you are alone or have a small, ‘war room’ kind of a team. When a monolith is relatively small, deploying it is not a big deal, and cookie cutter scaling does not seem too wasteful (assuming the monolith does not depend on in-memory state that is hard to distribute).

[...]

> Next time you are in position to pitch micro-services to a worried project manager or product owner, don’t forget that technology is really not what you are selling – you are selling a solution for process, governance, cost of operation and scalability issues, not a technology. You are selling the ability to fix a typo on a prominent page of your large system within minutes without touching the rest of the system. You are promising the ability to maneuver an oil tanker as if it was a canoe, in a world full of oil tankers.
