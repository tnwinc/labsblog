---
title: "The Stack is a Much Deeper Concept than GOSUB"
layout: post
tags: [functional-programming]
published: true
author: Charlie Flowers
mail: "cflowers@tnwinc.com"
summary: Major AHA moment about functional programming!
---

_(I just had an AHA! moment. I’m excited about it. It’s valuable and I want to share it. Warning: I USE CAPS a lot when I’m having an aha moment. If you think that’s childish or unprofessional, sue me. I could not care less. If you appreciate a powerful insight communicated enthusiastically, then read on!)_

I learned BASIC before any other language. So I’ve always thought the stack was “what it takes to allow for gosub.” And ever since then, as I learned many different languages, there have always been features that use the stack (mostly, function calls).

When I encountered functional programming, I thought, “Interesting, these folks decided to see what would happen if you rely on the stack for everything.” They got rid of mutable state. When they wanted to change a variable, they instead created a new stack frame with new value. (And don’t worry about Tail Call Optimization … it’s an optimization, not a concept).

I like functional programming and see the benefits of avoiding mutation. But I always admired the “hacky creativity” of the inventors of functional programming in that they were using the stack to acheive things it was not originally meant for. How resourceful!

BUT I WAS COMPLETELY WRONG, AND THAT WRONG IDEA WAS HOLDING BACK MY PROGRESS IN FUNCTIONAL PROGRAMMING!

I was wrong, because functional programming existed LOOONG BEFORE structural programming! Structural programming (which GOSUB is a big part of) came about in the 1960’s, partly propelled by Dykstra’s famous “GOTO Considered Harmful” paper. But functional programming WAAAY predates it, going back AT LEAST as early as Alonzo Church and Lambda Calculus in the 1930’s! Yes, the THIRTIES.

Functional programming relies on the stack because Alonzo Church figured out a way of REASONING, BEFORE THE FIRST COMPUTER WAS EVER INVENTED! The way of reasoning that Church created relies on step-by-step transformation of an initial “formula” into an “answer” using concepts of “function application” and “substitution”.

He did not invent it in order to take advantage of the stack (kind of like how we build parsers based on recursive descent to take clever advantage of the stack). THERE WAS NO STACK YET! THERE WAS NO COMPUTER YET!

He invented it because it was a POWERFUL, PRECISE WAY OF THINKING. It fell under the umbrella of FORMAL MATHEMATICS, which is really just a collection of powerful, precise ways of thinking that help people reason out certain difficult problems.

He had never seen spaghetti code, or mutliple threads clobbering a mutable variable. He was free to reason through the problems however he wanted to. But he came to prefer an approach that he felt was PARTICULARLY POWERFUL, which involved a “stack-like” series of transformations (aka, “function application”). It wasn’t a compromise to him. It was empowering.
When you come to functional programming via structured/imperative programming (as I did and probably most programmers did), it’s easy to feel like the claim is, “If you stop using some of the core features you’re used to, you’ll be better off.” If you’re open minded, you’re willing to try it.

But the real claim functional programming makes is much stronger: “This is a more powerful way to think. You will be able to handle more complexity, and reach solutions faster, and maintain them more easily, because this is a more powerful way to think.” And it was this misguided notion of where the idea for a stack came from that was blocking me from seeing that.

_(Edit: By the way, I was working through “The Little Schemer” in Clojure when this insight hit me.)_
