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

The dynamicaly dispatched nature of Objective-C offers some interesting tools to help though. Using `performSelector:` it is fairly straightforward to add a second dispatch for `prepareForSegue:sender:` that is cleanly testable and, as a bonus, doesn't require a series of `ifs` or a `switch` statement.

<script src="https://gist.github.com/brendanjerwin/5677203.js"></script>

Using this method, I can easily write a test for the `entityListSegueToController:WithSegue:` method:

<script src="https://gist.github.com/brendanjerwin/5677228.js"></script>

### Memory Warnings
You'll notice the bits telling the compiler to ignore warnings: `#pragma clang diagnostic ignored "-Warc-performSelector-leaks"`.
Since the compiler can't determine the name of the selector being called at compile time, it is unable to inject appropriate `retain`, `release`, or `autorelease` calls on the return value of the mthods. (A portion of what ARC does is actually based on common naming conventions for certain methods.)
Because of this, it is important that the mthods being invoked never return any objects: they must always be `-(void)` methods.

### Tools Used
The unit tests are written with [Kiwi](https://github.com/allending/Kiwi) and I'm using the [OCMockito](https://github.com/jonreid/OCMockito) mocking framework.
Kiwi provides a really nice, nested context, style which I find meshes very well with the test-driven workflow.
OCMockito offers a low-ceremony approach to testing fakes. Notice that the only mention of the method in question was during the assertion. All OCMockito mocks are 'nice', so you don't have to go through a lot of pre-verification setup.
