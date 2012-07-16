---
layout: post
title: "Object.prototype.toString across windows bug in IE"
tags: []
author:
mail:
published: true
summary:
twitter-handle:
---
{% include JB/setup %}

It's never a dull day when building a thick-client javascript
app which needs to run in that lovely beast called Internet
Explorer.

We recently needed to expand our app's capabilities to work across
multiple windows. One of our goals in designing the interface to this
feature was to reduce, as much as possible, the need to care that the
view controller you are rendering will be rendering in another window.

What we landed on is that the new window would go through a small
bootstrap phase where it would return a reference to it's version of
`require` to the parent window. To execute code in the child window, one
just needs to use the child window's require instance.

{% highlight coffeescript %}
  dialog.window (domRoot, close, innerRequire)->
    innerRequire ['some_view_controller'], (SomeViewController)->
      SomeViewController.render
        el: domRoot
{% endhighlight %}

While this worked wonderfully in browsers like Chrome and Firefox,
Internet Explorer 9 refused to execute the callback function passed to
`innerRequire`.

After a good bit of tracing through the requirejs source, it all came down
to code similar to the following:

{% highlight javascript %}
  function isFunction(it){
    return Object.prototype.toString.call(it) === '[object Function]';
  }
{% endhighlight %}

Interestingly enough, Internet Explorer reports function and array
references constructed in other windows as `[object Obect]`.

The fix was to override `Object.prototype.toString` in the child window
to consult the parent window when the result equals `[object Object]`

To be quite fair, though, the Internet Explorer development tools have
come a long way in providing us with the tools we need to
track down these types of problems. I hope they continue to invest in
this area, as it really helps to lower the barrier to making our
applications run properly in IE.
