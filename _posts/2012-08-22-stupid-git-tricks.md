---
layout: post
title: "Push Me, Push You"
tags: [git, tricks]
author: Ricardo Diaz
mail: ricardodiaz@tnwinc.com
published: true
summary: Save a few keystrokes with this push trick.
twitter-handle: cubanx
---
{% include JB/setup %}

code code code

commit commit commit

    $ git push origin some-branch
code code code

commit commit commit

    $ git push origin some-branch

If I have to type the origin name and the branch name one more time, I'm going to blow a gasket.

Save some keystrokes, do this:
    $ git config --global push.default upstream

This will make this:

    $ git push

Do the same thing as the above commands, you know, assuming you're on some-branch :)

There are other options you might like better for "upstream" above, you can read all the gory details about this on the [git-scm] page.

[git-scm]: http://git-scm.com/docs/git-config