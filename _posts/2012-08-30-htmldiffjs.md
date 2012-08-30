---
layout: post
title: "htmldiff.js"
tags: [projects]
author: Brendan Erwin
mail: brendanjerwin@gmail.com
published: true
summary: htmldiff.js - Diff algorithm that understands HTML
twitter-handle: @brendanjerwin
---
{% include JB/setup %}

I'm pleased to announce a new project we've recently released to the
world: [htmldiff.js](https://github.com/tnwinc/htmldiff.js)!

It's a diff algorithm that is designed to work in the browser, with
HTML. It can also be used server-side with Node.js and is hosted in
[npm](https://npmjs.org/package/htmldiff) as a Node module.

Our htmldiff.js started out as a port of
[htmldiff](https://github.com/myobie/htmldiff), written in Ruby. We've
already enhanced it slightly though.
Unlike most of the diff implementations we found, ours (and the source
we ported) understands HTML and will avoid chopping up tags into invalid
HTML.

It does this in two important ways: first, it tokenizes the input data
not just into words, but also into tags. Second, it keeps the rules of
HTML in mind while generating the output. It will not mess up the
nesting of the tag structure while inserting `<del>` or `<ins>` tags.

We use this code in our product to show our users differences in their
documents. It has been tested in IE8 and 9, Firefox, Chrome, and Safari.
