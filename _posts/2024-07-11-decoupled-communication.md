---
title: Decoupled Communication Between Services
date: 2024-07-01 12:00:00 -0700
categories: [Software Engineering]
tags: [code]
---

## Decoupled Communication Between Services

## Introduction

When two services, let’s call them Frontend Service and Backend Service, are communicating with each other, the most straightforward way is for one to call the API endpoints of Backend Service. This synchronous call is usually accomplished by making that RESTful call to the service or by using a client package that the receiver service has in order to facilitate those API calls.

This works fine, Frontend Service makes the call directly to Backend Service and gets the result back. The problem with this is that now Frontend Service is tightly coupled to Backend Service; if we have to deploy to Backend Service, and Frontend Service calls it, there will be an error in Frontend Service. Obviously we can go into maintenance mode and prevent any calls from happening, but why should we stop functionality of Frontend Service if we’re only deploying Backend Service?

## Message Queue (or Messaging System or Message Broker….)

A good solution to this problem is by introducing a Messaging System between both services. This means that Frontend Service won't be talking directly to Backend Service, it will instead send a message to the messaging system on which Backend Service will be listening. 

By adding this new layer, Frontend and Backend Services are now more loosely coupled. Each service can be in maintenance mode and won’t affect their functionality; the messages will be queued until the listener service comes back up and starts processing requests again.

There are many tools to accomplish this Azure Service Bus, Kafka, RabbitMq, among many more.

## Scenario

Recently, I was working on a web project that talks to a backend service. I decided to use Kafka, just to see how it works. Confluent Kafka makes it really easy to set up a cluster, which is a grouping of functionality, among them topics (called queues in other systems). I won’t go into the details of how to set it up, their tutorial is really good.

What I did find out during my development is that some calls can be done by sending it to a message queue since they are asynchronous, but others still have to be synchronized, since the user is actively waiting to see a response.

In this ranked choice voting app I have four main actions, that are synchronous:
Get Candidates for an Election
View Results
Submit Vote
Create new Election


## Making Calls Async

Out of those four actions, I can make `Submit Vote` and `Create new Election` asynchronous as the user is not actively waiting for a response. Showing a message that the vote or new election was successfully submitted is good enough.

So, now instead of calling Backend Service to submit the vote, I updated Frontend Service to be a producer to Confluent Kafka. This means that when the user clicks submit on those actions, it will instead send a queue message to a Kafka Topic specified in the app settings. 

On the other side of the process, Backend Service now has a Kafka Consumer Background service, that is actively listening to any new messages on the same topic. Once it sees a new message it will begin processing the message.

Besides changing how the services interact with each other on those actions, I had to make another change: Generate the IDs on the Frontend Service, this way we can show the user the URL to view the results of their votes.

The way I coded the Backend Service, I didn't have to do much rework, since Kafka Consumer runs as a background service it doesn’t impact the regular endpoints. And since there are different layers, I was able to inject the business service to process the votes or new elections, without having to change the rest of the functionality.

Because of this decoupling, the processing of a new vote or new election can take longer and it won’t impact the usability the user experiences.

## Conclusion

This is a very simple and straightforward way of using messaging. It decoupled some part of my system to allow changes to happen separately, but not everything can be made asynchronous. 

Message Queues offer a lot more functionality than what I talked about here. There is overlapping functionality, but i’ll list out the main focus on each of the brokers I mentioned:

Message streaming (this is where Confluent Kafka is best)
Message Routing, RabbitMq is strongest here. A message can be directed to a different queue based on a key)
Azure Service Bus is best of Publish/Subscribe, in which many consumers can subscribe to one queue.

As I delve deeper into message brokers I’m sure I'll find more things to focus on, like reliability, “at least once”  delivery guarantee, events and telemetry.
