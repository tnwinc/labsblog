---
title: "Text Generation Using Statistical Distributions from Sample Data"
layout: post
tags: [text, procedural, generation]
published: true
author: Adam Moss
mail: "adamrmoss@gmail.com"
summary: "Quickly generate large sets words that bear a passing resemblance to provided sample words.  The results are usually pronounceable!"
---

We found that we needed to generate some large XML datasets in order to create text fixtures for about bad-ass dimension import.
Rather than spend too much time on this totally useful task, I instead elected to obsess about generating names similar to those
in a sample set, even though "Dimension1", "Dimension2", etc would have been perfectly acceptable.  So then the question was,
"How to represent the characteristics of our sample way in a manner that would allow us to reproduce them?"

So I went with a very simple approach that doesn't bake in any assumptions about English grammar.  Rather, we chunk the word into
subwords, specifying a minimum and a maximum subword size.

