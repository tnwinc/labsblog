---
title: Adventures In Chocolatey Script Machine Setup
layout: post
tags: 
  - chocolatey
  - setup
  - suffering
  - bootstrapping
  - powershell
  - productivity
published: true
author: Chris Kelly
mail: "chriskelly@tnwinc.com"
summary: "Lessons learned, people cursed, and adventures in setting up a new development machine using a Chocolatey Script."
---

### What is Chocolatey
Chocolatey is a global PowerShell execution engine using the NuGet packaging infrastructure. Think of it as the ultimate automation tool for Windows.  Chocolatey is like apt-get, but built with Windows in mind (there are differences and limitations). For those unfamiliar with apt/debian, think about chocolatey as a global silent installer for applications and tools. It can also do configuration tasks and anything that you can do with PowerShell. The power you hold with a tool like chocolatey is only limited by your imagination!  You can develop your tools and applications with NuGet, and release them with chocolatey!
But chocolatey is not just for .NET tools. It's for nearly any windows application/tool!


In more human/less techy words, Chocolatey is a collection of software that you can call to download in install inside of a script.  It will automatically download and then install the software to your machine.  Thus saving you the time of finding the download, downloading it and then installing it to your machine.



###Mixed Reviews on use

After receiving my new machine, I heard mixed reviews on a Chocolatey script that was developed to install all of required software needed for developing code at The Network Inc.  I got everything from it works perfectly on my machine to it blew up my machine to it works great if you run each step one at a time.   Despite my concerns on the mixed reviews I decided to dive right in and try using the script, running it one step at a time.  After a number of false starts, I was able to use and tweak the script to get almost everything up and running on my machine.  I would call it overall a Success with a couple of bumps and bruises along the way.


###Diving in head first

First things first, I took a Restore point of my system in its current state.  Just in case I had to rollback and start over (which came in handy one, two or three times)

After that it was time to dive right in, first thing to do was install Chocolatey on my maching and then from an elevated command line (Administrator) open a powershell command window with no profile and unrestricted execution.

So far so good, time to get to the meat of the script.

I started moving through the script one line at a time, going well until I got to the ruby install.   While it installed cleanly, I was unable to get any of the ruby commands deeper down in the script to fire.  It was like Ruby was not in the enviornment path for the machine, even though the script placed the correct bin directory into the path.   After researching, we found that the cinst ruby command for Chocolatey was installing the newest version of Ruby (2.0), and we were expecting Ruby version 1.9.3.  Turns out someone had asked for the version of ruby to be updated in the repository and this was then causing issues for us.   We found the command to install a particular version (cinst ruby -Version 1.9.3.44800).   After trying to uninstall the 2.0 version and reinstall the 1.9.3 version multiple times, it was time to rollback to that restore point talked about in the first line.


###Attempt 2, Armed with the correct version of Ruby

Okay time for attempt number 2.  Starting again from the top, I was able to this time get the correct version of ruby installed.  I kept moving through the list and found that my ruby commands were still not working.  In looking at what was going on, I found that I still have both versions of Ruby installed (1.9.3 and 2.0.0). How is that happening, I told the script exactly what version of Ruby to install.   In looking back at the output from each command, I noticed that when installing the Ruby Dev kit from chocolatey it was taking a dependency on Ruby 2.0 and thus reinstalling it for you.   Back to the restore point we go.

###Attempt 3, This time it has to work, Right?

Back to the beginning again.  Third time has to be a charm.  Back to running the steps, this time instead of using chocolatey to install the Ruby dev kit, installing it manually from Ruby.  All looking good, getting through all of the rest of the install steps now. Made it all the way down to doing npm install on the client directory, and then we fail again because the system does not know what npm is.  How did this happen?  That should have installed with nodejs.  How did it not happen?  Well it turns out that cinst nodejs only installs node and does not include npm.  Instead you have to run cinst nodejs.install.  That will install both node and npm.  Time to rollback and try again.

###Attempt 4, Gotta get it this time.

Running through everything again, using the lessons learned from the first three attempts.  Running all the right commands.  HEY, it is looking good. Running the npm stuff.  Damn, Python is missing.  Easy fix, run cinst python.  Rerun, hey I am able to finish the npm install.  Go back a level, do a rake build.  Everything builds.  Awesome, things are working.

###Whats Next

Although you are ready to run, you still have to do any of your configuration setup, git hub alsis, git hub user name and name, resharper tweaks, etc.   But nothing too bad. You could start writing spec flow, running the suite locally and running test locally.

###Overall feelings
Still undecided on if this way was easier or not.  There was a lot of trial and error, rolling back to the check point, and calling over the developer who designed the script to figure out what is going wrong.  It did get everything installed and running.  Did it do it faster than downloading and installing each item by themselves?  I think that is still up in the air.  I guess time will tell as someone else tries to use the script.  Hopefully the tweaks I have in the comments, help out the next time.
