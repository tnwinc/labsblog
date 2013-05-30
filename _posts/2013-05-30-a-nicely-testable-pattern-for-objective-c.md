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

<script src="https://gist.github.com/brendanjerwin/5677203.js"></script>

Using this method, I can easily write a test for the `entityListSegueToController:WithSegue:` method:

<script src="https://gist.github.com/brendanjerwin/5677228.js"></script>

