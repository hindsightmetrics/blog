---
title: In MY Utopian Project...
date: 2026-02-08 12:00:00 -0700
categories: [Software Engineering]
tags: [clean-code, pragmatism, architecture, technical-debt]
---

## Introduction

Most software engineers have in their minds a perfect system, with the strictest conventions, optimal performance, extremely easy to read and maintain. It is from this **utopian project** that we develop (no pun intended) the *rules* we follow when building a new system, maintaining an existing one or when reviewing other people’s code.

## The Walls

In my experience there are a few walls that I’ve hit to learn that *the rules* that I’m following cannot always be applied. I’ll be talking about the ones I’ve experienced, but I'm sure that there are others that I don’t realize.

### Existing code

Getting to existing code you have to adapt to the patterns that have been decided on. It’s very common for a junior developer to think they completely rewrite an existing component because it doesn’t follow *the rules*; I know I tried. This is the first time I had to let go of having “perfect” code, I had to accept that there were functions that were very long, or improperly named functions or sub-optimal performance.

#### Why is it so hard to improve an existing system?

There are a few reasons, but mainly: priorities. Any attempt to improve something takes up Dev Time and resources (QA Engineers, deployments, automation, etc.) that can be used to build new features or fix bugs. In this scenario what I suggest is to create a *Tech Debt* task, and work with project managers to get it prioritized.

### Third party dependencies

Another thing that can impact *the rules* is when we have to integrate with an outside system or a third-party project. Some integrations are very opinionated and those opinions have to be followed for the best performance, even if those conventions are different than our *rules*. For example, if we want to integrate with Kafka we now have to consider how Kafka works and how it expects to receive those messages.

#### So, how can we handle this?

Here we can “contain” all the integration, basically we can wrap all those dependencies into their own local projects or packages. This allows us to keep all of *their rules* in that specific area without it impacting *our rules*.

### My *rules* are different from my teammates’

This is one of the toughest to encounter. We all come from different experiences and (hopefully) have the best intentions, but it can get difficult if my *rules* are different than my teammates.

#### What did I learn?

Well, first of all I had to understand that we both are looking to accomplish the task in the best way possible. If we’re both passionate it means we care for the success of the project.

Second, I had to learn to pick my battles: what do I care about the most?
If I start pointing everything out just because it’s different from my **utopian project**, no one will want to work with me.

Then, once I picked a battle, I led by example by showing why I think my *rules* are better. Every decision has pros and cons, even if I considered those cons to be minimal.

Lastly, and this was hard to get over, even after showing proof that what I believe is better I had to understand that my teammates might not agree and had to accept that my *rules* weren’t accepted.
