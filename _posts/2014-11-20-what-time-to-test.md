---
title: What Time to Test
layout: post
tags: 
  - "testing"
published: true
author: Steven Vore
mail: "StevenVore@tnwinc.com"
summary: "Know your time zones and use them all."
---

{% include JB/setup %}

I've been workng on a new feature with a few other devs, and we're eager to get it done and into our master branch so that it can be deployed sooner than later. To that end, after dinner and some shopping last night, I picked up my laptop and thought I'd get a bit more work in while my wife was busy on a project of her own. 

Since we've got multiple people on this branch, and multiple teams across the app, I try to always start off the same way: `fetch` from our 'origin' repo on github, `merge` in any changes to this branch, `merge` in any changes to master, then run all the unit tests. Since all our devs subscribe to the same philosophy of fixing failing tests quickly, any problems that come up are usually caused by something new, be it in the branch or someone else's recent changes to master. 

I was surprised, then, to find a failing test in a section of code that wasn't new &mdash; how had that cropped up? I checked our CI server, and it showed the green on previous builds. How did this come to be? 

I pointed it out to another dev on my team, as this was a section of code I wasn't as familiar with, and he quickly realized what the problem was. We were fortunate enough to have found it _purely_ because of timing &mdash; if we'd finished eating dinner more quickly, we may not have found it! 

In our applications, all date/time values are stored in UTC. In one place in our backend code, where making a decision which course of action to take based on the number of days overdue a task was, a dev had accidentally used `DateTime.Today.Date` instead of `DateTime.UtcNow.Date`. During the day, no problem. In this case, I had happened to run the unit tests beween midnight UTC and midnight local time, so "now"; was no longer "today." The app was trying to send a "30 days overdue" message when in reality the task was still only 29 days late. 

Two lessons. The first, of course, is to have good unit tests that can be run easily, quickly, and often. The second &mdash; the one we learned last night &mdash; is that it might be a really good idea to be run at various times throughout the day, not just while people are in the office. 

