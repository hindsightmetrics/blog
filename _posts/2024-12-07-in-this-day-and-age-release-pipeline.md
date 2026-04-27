---
title: In this day and age, do we really need a release pipeline?
date: 2024-12-07 12:00:00 -0700
categories: [DevOps, cicd]
tags: [pipelines]
---

## Introduction

Why do we have different build pipelines per environment? Recently, I started to question why can’t we build everything in a single pipeline. At the end of the day, the build pipeline can create artifacts from any branch, and it’s up to the deploy system to push those artifacts to the appropriate environment.

Having a pipeline per environment feels like a solution that stuck from when the build systems weren't as dynamic, and had to be locked in to a branch. With this legacy system, release pipelines don’t get exercised as much as the development pipelines. Things are going to fall through the cracks.

We are now in a world where configuration is king and everything is a parameter. If the build pipelines are only building artifacts, any configuration or variables can be set based on the branch pattern (e.g. `dev`, `release/`*, `hotfix/`)*. Afterwards, it’s up to the deploy pipeline (or server) to move those artifacts to the appropriate environment.

With a single pipeline there are other challenges, hopefully the solutions are not too difficult. I’ll list out some of the challenges I see.

- What happens when we need to make a change to the pipelines?
  - Updates to the pipeline are inevitable (like changing the `dotnet` SDK or changing a connection string). For this the pipeline can be versioned: V1, V2, V3, etc. This means that we would have two pipelines for a bit, until the code that needs the new change makes it to Production, after this, the old pipeline can be archived.

- Who controls Build Numbers?
  - In my experience, build pipelines add the build numbers based on a configuration, build counter and/or commit hash. All these can still be set the same way, but now they are based on the branch pattern.

- If you have a single pipeline, can you have concurrent builds?
  - Yes. In all systems I've worked on (Azure DevOps, TeamCity, Jenkins), you can run the same pipeline multiple times at the same time.

- How does CI/CD fit in a single pipeline?
  - It’s all the same still. The pipeline can be configured to automatically build if there is a commit to the Dev Branch, but only manually build from the release branch.

- What if I have different steps for each environment?
  - Since it’s based on branch pattern, steps can be included or excluded. This can happen in case of deeper analytics or notifications to other teams.

- What about compliance?
  - Sometimes there are rules or regulations that code or artifacts have to be segregated completely. If these exist, I don’t see a way around this, multiple pipelines are required.

I don’t see a valid reason to keep maintaining multiple build pipelines. I feel that we kept multiple pipelines just because that’s what we had before, I may be missing something or haven’t thought of a good reason to keep multiple pipelines.

I’ll keep thinking about this….
