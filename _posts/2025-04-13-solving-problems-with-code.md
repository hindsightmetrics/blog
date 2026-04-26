---
title: Solving Problems with Code?
date: 2026-04-13 12:00:00 -0700
categories: [Software Engineering]
tags: [optimization]
---

As a Software Engineer, my instinct is to solve problems with writing code. Obviously, that’s the “software” part, but lately I've been thinking about the “engineer” part derived from the Latin word “ingenium” which means cleverness or ingenuity.

This came to mind recently with the [Sleeper API](https://docs.sleeper.com/), the best app for Fantasy Sports:

> Initially, I wanted to develop a console app (or a private API) with a database behind it. It would call the Sleeper API once a week to get all the stats, scores and matchups for the week, save them into an excel or CSV file, to then run a series of formulas to get different metrics. But a friend of mine has a simpler solution.
> Use the Google Sheets [Data Connector](https://dataconnector.app/), to import the data into one tab, and have all the formulas in a separate tab that references the first tab. The only thing my friend has to do is invoke the data connector.

Don’t get me wrong, working on that app would’ve been fun, but there are a lot of moving parts that would have made everything more complicated. My friend’s solution is easier to maintain, more straightforward and quicker to complete.

This made me realize that a new application is not always the optimal solution. Now, I try to take a step back and find the easiest way to get the job done.

For example, I had a situation where I needed to call an API I made every 5 minutes, sure I could set up a [Hangfire](https://www.hangfire.io/) background service, or have something like an [Azure Function](https://learn.microsoft.com/en-us/azure/azure-functions/functions-overview), but after some research I found a tool called [UptimeRobot](https://uptimerobot.com/), I set it up to call the endpoint (with some headers) and call it day.

With these experiences, I’m reminded that software engineering isn’t only about writing code, it’s about solving problems… and sometimes the best solution is the simplest. With all this, my mindset has been evolving from “Solving problems with code” to “Solving problems”.
