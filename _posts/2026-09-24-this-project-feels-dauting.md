---
title: What to do with a Daunting Project
date: 2026-09-24 12:00:00 -0700
categories: [Software Engineering, Design Patterns]
tags: [software engineering]
---

## Introduction

Some projects can feel scary due to their size or moving parts, and the feeling gets worse when you begin to design them. It happened to me recently; I was designing a new project and while adding more boxes, databases, arrows, etc., I started to feel that completing it was unreachable. Daunting.

## What to do?

**First of all, take a breath.** It's easy to get lost in all the boxes and connections, but, remember, you don't have to do everything all at once; it's an iterative process. The best thing to do is to identify the box in the diagram that has the core of the functionality.

Here's an example: let's say I'm going to design a tagging system, similar to a hashtag[^1] on Instagram. We could design it in a way that's performant with proper microservices, queues, data stores, caching, permissions, and more things I'm not thinking of right now. It's easy to get lost in the details and all the connections. But what really happens when someone adds a hashtag?

## Focus on the core

In our hashtag example, what is the main function? It's a way to group content from one or many users by that tag. Now that we have the main functionality, there are many approaches to accomplish this (create a search term for each hashtag or create a hash table of the hashtag with related content).

Now that you know what the core of the functionality is, you can focus on that and get it working, even if it's just with basic API calls; you can add a UI around it later. Once you've got that core functionality working, you can focus on performance.

## Add supporting services

Now you can add all the supporting services or infrastructure. Do you want to add caching? Go ahead. Need a queue to optimally process many hashtags at the same time? Good!

[^1]: I know that the term originally came from Twitter.
