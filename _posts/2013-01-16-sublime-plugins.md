---
layout: post
title: "Spec Runner &amp; Coffee Cleaner for Sublime Text"
tags: [word, docx, html]
author: Chris Breiding
mail: chrisbreiding@tnwinc.com
published: true
summary: Plugins for Sublime Text for running specs with keyboard shortcuts and cleaning up CoffeeScript on save.
---

{% include JB/setup %}

Here at the DevLabs, Vim is a very popular text editor. My fellow developers have created a couple plugins for it that are integral to our front end development workflow. [One](http://labs.tnwinc.com/2012/07/14/vim-spec-run/) allows running the front end test suite in different ways using keyboard shortcuts. Another cleans up a CoffeeScript file's formatting when it is saved.

Vim is awesome, but, personally, I prefer Sublime Text. So when faced with the choice of switching to Vim in order to utilize the aforementioned plugins or write equivalent plugins for Sublime, I chose the latter course.

The [Spec Runner](https://github.com/tnwinc/vim-spec-runner/tree/master/sublime-text) plugin allows you to set up key bindings to run specs. It's great being able to work on a spec and then quickly run it with a keyboard shortcut. One caveat: unlike the Vim version, it currently only works with iTerm.

The [Coffee Cleaner](https://github.com/tnwinc/vim-coffee-clean/tree/master/sublime-text) plugin will clean up and format your CoffeeScript on save.

I learned a lot writing both plugins. The Spec Runner was my first experience writing a Sublime plugin and also my first experience working with Python. The Coffee Cleaner put my knowledge of regular expressions to the test.

A lot of credit goes to James and Brendan for writing the original Vim plugins, off which I based the Sublime versions. So far, I've found both plugins immensely useful. I hope other Sublime Text users find them useful as well.