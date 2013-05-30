---
published: "false"
layout: post
title: "A Nicely Testable Pattern for Objective-C"
tags: 
  - "objective-c"
  - testing
  - iOS
author: Brendan Erwin
mail: "brendanjerwin@gmail.com"
summary: A nifty pattern to increase the testability of your controllers.

---

TDD in Cocoa Touch can be a bit of a challenge. The framework and the collection of common patterns that people use just havn't had testability as a goal for very long. `prepareForSegue:sender:` is one such case where the framework offers a hook that doesn't seem to have testability in mind.

The dynamicaly dispatched nature of Objective-C offers some interesting tools to help though. Using `performSelector:` it is fairly straightforward to add a second dispatch for `prepareForSegue:sender:` that is cleanly testable and, as a bonus, doesn't require a series of `if`s or a `switch` statement.

