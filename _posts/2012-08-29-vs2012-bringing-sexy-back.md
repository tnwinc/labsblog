---
layout: post
title: "VS2012 bringing sexy back"
tags: [ide, productivity]
author: Ricardo Diaz
mail: ricardodiaz@tnwinc.com
published: true
summary: VS2012 has some new hotness
twitter-handle: cubanx
---
{% include JB/setup %}

Finally got a chance to upgrade to VS2012 today. Since ReSharper is official, and already on 7.01, I thought I should get to it.

Let's get this out of the way, the SCREAMING MENUS are unpleasant. I have a fix for that below. Besides that though, I like the flat look, but that's just me. The Dark Theme looks great with [Solarized].

First, the upgrade experience. It could NOT have been smoother. During install, I pared it down to only VS2012 and Web techs since we use MVC here.

I then opened our teams' Solution file. Edited a few files and saved, and the only files that changed were the ones I touched. I know this might not seem like a surprise, but it left the 2010 solution format intact, and that's important for a smooth upgrade experience.

###VS 2012 Improvements

1. Preview Selected Items
1. Class Breakdown built into Solution Explorer
1. Built in line highlighting
1. Find menu much better (works like [Productivity Power Tools])
1. Multi threaded builds
1. Seems to be smarter about what has changed when you build
1. Dark Theme!
1. Quick Search (works like [Productivity Power Tools])

###VS 2012 Extensions I use
1. [VSCommands] - This adds some nice bits to the build output, adds a nice badge/hover effect when you hover on the Win 7 taskbar, and most importantly it can **MAKE THE MENU STOP SCREAMING**
1. [Gister] - This lets you highlight a hunk of code and throw it up on http://gist.github.com
1. [CancelFailedBuild] - This stops the build the moment it sees an error. Handy to get the feedback loop as short as possible.
1. [HideMainMenu] - Hiding the main menu is built into VSCommands, but it doesn't hide the new Quick Search bar. This extension hides the whole thing.

So far, this has worked out pretty well. Here's a screenshot of my setup running in full screen mode. ![VS2012 full screen](/screenshots/vs2012-sexy-back/vs2012-full-screen.png "VS2012 full screen")

All these extensions are available in 2010.

One of the extensions I did NOT install, which I had in 2010 is the [Productivity Power Tools]. This was for 2 reasons.

1. It's not available for 2012, and it may never be because...
1. A large part of its functionality is built in 2012.

Pinning windows, improved Find, Quick Search, etc are all built in to 2012, and they're a big part of the reason 2012 is better.

Overall, I wholeheartedly recommend you upgrade right away if you can!


[Solarized]:https://github.com/leddt/visualstudio-colors-solarized/tree/master/vs11
[VSCommands]:http://visualstudiogallery.msdn.microsoft.com/a83505c6-77b3-44a6-b53b-73d77cba84c8
[Gister]:http://visualstudiogallery.msdn.microsoft.com/b31916b0-c026-4c27-9d6b-ba831093f6b2
[CancelFailedBuild]:http://visualstudiogallery.msdn.microsoft.com/92a14c1d-82ce-409d-8e16-3f2aac0a00ea
[HideMainMenu]:http://visualstudiogallery.msdn.microsoft.com/bdbcffca-32a6-4034-8e89-c31b86ad4813
[Productivity Power Tools]:http://visualstudiogallery.msdn.microsoft.com/d0d33361-18e2-46c0-8ff2-4adea1e34fef