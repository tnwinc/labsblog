---
layout: post
title: "Syntastic Now Has CoffeeLint Support"
tags: [vim]
author: Brendan Erwin
mail: brendanjerwin@gmail.com
published: true
summary: "Syntastic has been updated to integrate with CoffeeLint"
twitter-handle: brendanjerwin
---
{% include JB/setup %}

I wrote a slight improvement to the [Syntastic vim plugin](https://github.com/scrooloose/syntastic) recently. The pull-request was accepted, I guess it will be in the next release.

It now runs [CoffeeLint]() if you have it on your `PATH`. It respects
the same configuration options for CoffeeLint as expected by the
[vim-coffee-script](https://github.com/kchmck/vim-coffee-script/)
plugin, so using them together should work great.

Here is a little video of it in action:

<iframe width="480" height="360" src="http://www.youtube.com/embed/rNH3OZNSsog?rel=0" frameborder="0" allowfullscreen></iframe>
