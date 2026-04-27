---
title: Relational Database vs NoSQL (Document Store)
date: 2024-07-17 12:00:00 -0700
categories: [Software Engineering, database, nosql]
tags: [code, datastore]
---

## **Introduction**

Recently I started a project to help my wife with something of hers. Like most tools online it needs data storage. By default I always think of Relational Databases: I know how it works, how to set it up, I like using [FluentMigrator](https://github.com/fluentmigrator/fluentmigrator), I like the strict structure… or so I thought.

## **Relational Databases**

The problem here is that the schema, structure of the data, is contained within the RDBMS (Relational Database Management System) so, we have to do mapping from the database objects to objects in code, usually called DTOs (Data Transfer Objects). If a new field is needed in the schema we have to go in and update the DTOs to match the new changes. This causes tightly coupling which is bad because one change can impact other systems and would need to update too.

There are tools that help with this like [Dapper](https://github.com/DapperLib/Dapper) or [Entity Framework](https://learn.microsoft.com/en-us/ef/), but still the source of truth comes from the database schema\*.

## **Enter NoSQL Document Store**

NoSQL has many offerings, but iI want to focus on the Document Store. Most of the time a document store is based on JSON, and there’s not strict schema\*\*. This allows the code to control how and what is stored, by serializing a class into json and sending it over to the store.

This is a big shift in paradigm, now the code and the data are more closely connected, it gives developers more control while coding versus having to go back and forth between an RDBMS and code to make changes and allows for more incremental changes in the data structure.

Interestingly, this sounds like we’re tightly coupling the code and the data, but here it’s different, since the Document Store saves the serialized objects however they are sent, it makes it very flexible.

## **Way more to consider**

I have only focused on the developer aspects of each approach but there is a lot more to consider. Things to consider can be type of project, scalability, data consistency/flexibility or even team formation among many other things

### **Type of project**

If you’re working on a project where you would need a lot of reporting or complicated queries, the best solution is to use a Relational Database. A NoSQL database doesn’t have the best tools to make complex queries or they can get very difficult very quickly.

Maybe a project might need more granular constraints on data, like role based access control on certain fields, or need a clean separation between code and data, that allows each one to be changed separately (as long as the contracts don’t change).

### **Scalability**

NoSQL excels in scaling by adding servers (horizontal scalability), since it works by individual documents and the engine splits the information into different servers, and it knows how to best find the data by using query routing or load balancing.

By contrast, a Relational Database is usually contained within one server, with mirrors and backups, but it is not able to scale horizontally as easily because the engine keeps track of things within that main server.

### **Data Consistency/Flexibility**

Data consistency obviously goes to Relational Databases. The fixed data structure, while laborious to set up and get to the precise set up needed, makes the data be more consistent since everything has to follow the structure.

On the other hand, if you need to add a new field to a table or a new foreign key relationship in a Relational Database you have to take a lot more steps to set up (new table, update your DTOs, update stored procedures if you have them, check constraints). But on a NoSQL you only make a change to your DTO and call it a day

### **Team Formation**

If you have a Database Administrator Team, you need to give them tools to manage and optimize the performance, create backups or monitor the database. Of course the same can be done in a NoSQL database but since the focus is different, DBAs would need to be trained on how things work in this NoSQL world.

## **Conclusion**

It’s good to know that there are other types of data storage. Most of my development life I have used relational databases, but on this latest project I ventured more into Document Store and I really liked the flexibility it gave me. As I was adding functionality, I realized that I needed to have two classes related to each other and instead of having to deal with the schema to create a new table and foreign key constraints, I only had to include a class in another.

P.S. As I was writing this there was basically an exception to every rule or constraint I mentioned. Everything has a workaround and can be made to behave how the developer wants it. The difference is that some things are easier to do in NoSQL and others in a Relational Database.

\* Entity Framework has a Code first offering, but I haven’t used it.

\*\* Some NoSQL solutions allow you to enforce a Schema Validation.
