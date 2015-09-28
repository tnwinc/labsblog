---
title: "IE10 and \"Tweener\" Flexbox Frustrations"
layout: post
tags: [ie, css, flexbox]
published: true
author: Will Stamper
mail: "WillStamper@tnwinc.com"
summary: IE10's flexbox implementation is strange and buggy, but the workaround is simple and (perhaps) unexpected.
---

We've recently begun to use flexbox in our production code, and the experience has mostly been great. We use [Modernizr](https://modernizr.com/) to feature-detect flexbox support and use it where available. In an interesting twist, Modernizr 2.x [reports IE10 as supporting flexbox](https://github.com/Modernizr/Modernizr/issues/812), which is only somewhat true. As you may know, IE10 has an unusual implementation of flexbox, sometimes called the "tweener" or 2011 syntax. Basically, IE10 implements a prefixed version of a working draft of flexbox that was later significantly modified before being adopted as the current flexbox spec. As a result, IE10's implementation is pretty unique and (predictably) buggy.

Fortunately for us, we use [Bourbon](http://bourbon.io/), which provides us with a lovely [set of mixins](http://bourbon.io/docs/#flexbox) that includes full compatibility with the tweener version of flexbox. Using flexbox is (theoretically) as simple as `@include flex(1 1 auto)`. Unfortunately, reality and theory don't always match up, and it quickly became apparent that something was going wrong.

We have a list of items where the contents of each `<li>` include a span, a label, and a div. The contents of the label are dynamic, and we're using flexbox to expand and contract it to fill the available room as necessary. When the content of the label is too long for the available space, we'd like it to get truncated with an ellipsis ("...").

{% highlight html %}
<ul>
  <li class="first-item">
    <span>A</span>
    <label>This is a long label</label>
    <div>1234567</div>
  </li>
</ul>
{% endhighlight %}

{% highlight css %}
.first-item {
  @include display("flex");
  label {
    overflow: hidden;
    white-space: nowrap;
    text-overflow: ellipsis;
    @include flex(1 1 auto);
  }
}
{% endhighlight %}

In modern browsers, this looks [about how you would expect](http://codepen.io/epmatsw/pen/Lpbqxq):

![Firefox 43 :)](/screenshots/ie10flexbox/firefox.png "Firefox 43")

In IE10, this does not work how you would expect:

![IE10 :(](/screenshots/ie10flexbox/ie10broken.png "IE10")

After fruitlessly browsing through [Microsoft's IE10 flexbox documentation](https://msdn.microsoft.com/en-us/library/hh673531%28v=vs.85%29.aspx) as well as the first couple pages of a few different Google searches, I decided to open a [Codepen](http://codepen.io/epmatsw/pen/Lpbqxq) to try to tinker my way to a solution. The first thing I noticed was that, as expected, the label was not growing or shrinking at all. I tried a few different syntaxes of flexbox just to make sure it wasn't Bourbon being dumb, but the breakthrough came when I put the flex styles on the `<div>`. At that point, it immediately expanded to fill the available space, and that's when the lightbulb came on.

The key to the puzzle is that, unlike a modern flexbox implementation, IE10's tweener flexbox won't flex inline elements, including `<span>` and `<label>`. To make these elements flex in IE10, you can simply include `display: inline-block`. In our case, we didn't even have to put that behind a version check, and the problem was solved.

![IE10 :D](/screenshots/ie10flexbox/ie10working.png "IE10 (but working!)")

There's a [live Codepen demo here](http://codepen.io/epmatsw/pen/GpNPvP). Both versions should work in a modern browser, while only the second `<li>` will be formatted correctly in IE10.
