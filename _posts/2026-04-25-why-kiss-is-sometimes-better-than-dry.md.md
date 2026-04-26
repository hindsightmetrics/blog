---
title: Why KISS is sometimes better than DRY
date: 2026-04-25 12:00:00 -0700
categories: [Software Engineering, Design Patterns]
tags: [kiss, dry, architecture]
---

## Introduction

I recently worked on a project where the concepts of Don’t Repeat Yourself clashes with Keep It Simple, Stupid.

### Don’t repeat yourself

The goal of this principle is to not duplicate code. By doing so we have more control of where code is and make it so a fix or an update is automatically applied to wherever it is consumed. This is usually accomplished by inheritance and abstractions.

### Keep it simple, stupid

On the other hand, we also strive to make code simple to read and understand. If we introduce too many layers of abstraction the code starts to get more complex.

## Real World Scenario

While on a project, I had Class A and duplicated almost all of it into class B. The code followed the same pattern: prep for an API call, make the API call, then get the response back and map it back to a domain object. The target API does almost the same thing, only in a different context, thus its client expects a slightly different request.

![Abstract classes to represent difference in code](/assets/img/posts/kissvsdry.png)
{: .shadow}

The blue lines of the diagram are set up to call the API. This could be a prime example to apply DRY.

### Reducing Duplication / Adding Complexity

I could extract base classes and inherit that base functionality, or use Template Method design pattern. But as I started the work I realized that it was only setup code, while verbose, it’s only mapping values back and forth.

Is it work to increase the complexity to only save a few lines of code, that are only there for mapping? I decided to KISS and keep things lean than go DRY and add layers of complexity.

## So, When is it Worth it?

In programming there are two kinds of “work” the developer does:

- **building blocks** (i.e. domain logic). While working on the building blocks DRY becomes more important, domain logic should not be duplicated because it’s crucial to the goals of the project.
- **using those building blocks**. However, using those building blocks usually requires a lot of boilerplate code that it’s ok if it’s duplicated to keep things easier to understand.
