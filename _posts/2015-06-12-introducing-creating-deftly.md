---
title: Introducing Creating-Deftly
layout: post
tags: [jsx, jsfl, javascript, adobe, illustrator, flash, automation]
published: true
author: St. John Peaster
mail: "StJohnPeaster@tnwinc.com"
summary: "Creating Deftly is an open source project devoted to making automation of Adobe products easier with a unified API, chainable commands, and in-app gui tools."
---

{% include JB/setup %}

Most Adobe tools have a javascript api for automation. And all of them are rather verbose. Method naming conventions can very from app to app. Some apps are running old outdated versions of ecmascript. For example, Illustrator doesn't natively support `JSON.parse()/stringify()` or `Object.keys()` to name a few. Furthermore, most methods don't return anything useful, so chaining is out of the question. All of this makes for clunky code that can be hard to read and a pain to write.

Enter [Creating-Deftly.](https://github.com/tnwinc/creating-deftly)
![logo](https://github.com/tnwinc/creating-deftly/blob/master/resources/artwork/CreatingDeftly_Logo.png "creating-deftly logo")

Our project aims to unify concepts wherever possible in a consistent declarative api. For example, `openFile(path)` is a method you should be able to call in any application. Not just that, it should also return you the object for the file you just opened.

Here's an example in Illustrator &mdash;

    var doc = Ai.openFile('path/to/my/file.ai');

_that_ in Creating-Deftly is equivalent to the following in vanilla illustrator jsx &mdash;

    file = new File('path/to/my/file.ai', DocumentColorSpace.RGB);
    app.open(file);
    var doc = app.activeDocument;

_openFile will take an optional argument of 'CMYK' or 'RGB' but defaults to 'RGB'_

And that is a very simple example!

At the time of writing this, Creating-Deftly is still in it's infancy. We are starting to have pretty comprehensive coverage of Illustrator and Flash but there is still TONS of room for improvement.

Here are the key goals going forward.

> 1. Develop an easy way to install Creating-Deftly on mac & windows
> 2. Make Creating-Deftly test-driven, as well as exploratory/production tests
> 3. Define what the universal api methods should be
> 4. Use Creating-Deftly as a platform to drive new in-app GUI tools
> 5. Document, Document, Document!

tl;dr

Creating Deftly is an open source project devoted to making automation of Adobe products easier with a unified API, chainable commands, and in-app gui tools.

If you're looking for an easier way to work with Adobe's ExtendScript, .jsx, or .jsfl then consider using and contributing to [Creating-Deftly.](https://github.com/tnwinc/creating-deftly)
