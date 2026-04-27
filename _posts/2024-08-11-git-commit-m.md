---
title: "git commit \-m `feat: new blog post`"
date: 2024-08-11 12:00:00 -0700
categories: [Software Engineering, git]
tags: [commits]
---

## Introduction

I’m a huge fan of git. Throughout my career I’ve used many source control systems: Visual Source Safe, Team Foundation System (tfvc), [Subversion](https://subversion.apache.org/) (with [TortoiseSVN](https://tortoisesvn.net/)) and even the horrible [Rational Team Concert](https://jazz.net/library/article/87559). But it was git that made care for how code is stored.

But, what I want to talk about today is Commit Messages, from the point of view of Git.

## Why proper commit messages are important

A commit message should be short and clear with what the changes applied are. Let’s say you’re **looking for a commit where a variable was changed from int to a GUID**, and you have to parse through a list of commit messages like:

``` text
1110 Updated changelog.md
1109 Refactor
1108 fix broken build
1107 i hate this!
1106 this should work
1105 aaaaarg
1104 Changes Address, Person and User classes
1103 The issue happened because we couldn't parse the xml with a null value that we expected, so we added null checks to the parse on class ParseXml.cs
1102 update per bug
1101 qa found bug to fix
```

Where did the variable change happen? Most of these commits are doing a really bad job at explaining what has changed:

- **Commits from 1101 and 1102** don’t tell us what changed, it tells us why the change was needed.
- **Commits 1105 to 1107** show how frustrated a developer was.
- This information should be kept in the project management system.
- **Commit 1103** goes too much into detail about the change. The “how” can be viewed by seeing the source code.
- **Commit 1102** tells us which classes were changed showing the “how” but not “what”
- **Commit 1101** tells us a “what”, but “refactor” is too wide of a definition to be able to pinpoint the changes.

In order to improve this commit history we have to follow a standard established for commit messages.

### Commits should describe what happened, not how

This list doesn’t show what happened, making them useless to see. What can we understand from “update per” or “qa found bug to fix”. Better commits could be:

```text
1102 validate datetime user input
1101 add index check against length of list
```

Note: If you have a project management system that can sync a commit to a work item, it’s always a good idea to link the two.

### Commits should be short

Out of the list of commit messages, probably commit 1103 is the one that will be skipped the most. It's too long for the purpose of history of changes. Commits should be understood at a glance, a better message here would be:

```text
1103 add null check to xml parser
```

### Commits should not list files

If we want to see the files change, we can easily list it from the commit. Typing the files in the commit message is redundant.
From

```text
1104 Changes Address, Person and User classes
```

To

```text
1104 add new address property
```

### Comments should a complete this phrase

“If applied, my commit will \[Commit Message Here\]”. This makes the commit message understandable and short. Using commit 1105, the phrase “If applied, my commit will **aaaaarg**” doesn't make sense.

Applying this to the commits that the frustrated developer did,  it would look like this

```text
1107 update xml parser to use GUID
1106 change variable type
1105 update usage of address
```

### Semantic Commit Messages

The previous rules have been working great, but if we apply Semantic Commits (also called Conventional Commits) we can add a type to each commit message, making it even easier to see what happened when. There is a limited number of commit types:

`feat`: (new feature for the user, not a new feature for build script)
`fix`: (bug fix for the user, not a fix to a build script)
`docs`: (changes to the documentation)
`style`: (formatting, missing semicolons, etc; no production code change)
`refactor`: (refactoring production code, e.g. renaming a variable)
`test`: (adding missing tests, refactoring tests; no production code change)
`chore`: (updating grunt tasks etc.; no production code change)

You can see more [information here](https://gist.github.com/joshbuchea/6f47e86d2510bce28f8e7f42ae84c716).

If we apply Semantic Commit Message to our last two commits, they would look like:

```text
1110 docs: update changelog
1109 refactor: split long class into multiple subclasses
1108 test: fix broken unit tests
```

## Conclusion

By applying a set of rules or conventions to our commit messages, our git history looks cleaner and easier to parse. There are more advantages to working like this like: work is more properly contained by commit messages, changelog can be automated. But the main goal is to make our commit graph easier to read so we can find what we’re looking for. Below is the same commit history with all the rules applied.

```text
1110 docs: update changelog
1109 refactor: split long class into multiple subclasses
1108 test: fix broken unit tests
1107 fix:update xml parser to use GUID
1106 fix: change variable type
1105 chore: update usage of address
1104 feat: add new address property
1103 fix: add null check to xml parser
1102 feat: validate datetime user input
1101 fix: add index check against length of list
```

Speaking of, which commit had the change from int to GUID
