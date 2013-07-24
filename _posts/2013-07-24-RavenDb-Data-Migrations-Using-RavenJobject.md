---
title: "RavenDB Data Migrations Made Easy With RavenJObjects!"
layout: post
tags: [ravendb, migration, json, RavenJObject]
published: true
author: Randall Borck
mail: "randallborck@tnwinc.com"
summary: "Migrating Raven documents to match your object structure...without Script?"
---

In many cases, migrations are simple. Either you can do a lazy migration, or you may be able to do "Evil" patching as blogged about by Ayende [here](http://ayende.com/blog/157185/awesome-ravendb-feature-of-the-day-evil-patching). Unfortunately, I was not able to use this patching method, so I needed another way...Lazy migrations could work for this, but that requires the addition of a new property (which was going to add some complexity) and then requires that we touch every entity before removing the old storage mechanism anyway. Not removing the old property also causes object bloat, and that may be fine for some cases, but I wanted it to end clean by the time I was finished. Suppose you have a document as follows:

```
{
  ...
  "Results": [{..., "Answer":"RavenDB Rocks!"}]
}
```
    
Now introduce a change to the code that allows multiple answers per object. This problem is now beyond what the Patching API enables you to do. Enter the RavenJObject. Fetch a list of objects selecting only the FullRavenId and use the document session to load them into a RavenJObject enumeration (I recommend paging your requests, but that may be another blog post someday).

```
var ravenJObjects = documentSession.Load<RavenJObject>(fullRavenIdsList);
foreach(var ravenJObject in ravenJobjects) {
  if (ravenJObject["Results"] != null) {
    var results = ravenJObject as RavenJArray;
    if (results != null){
      foreach (var result in results){
        if (result != null && result.ContainsKey("Answer")) {
          var answers = new List<string>();
          answers.Add(result["Answer"].ToString());
          result["Answer"] = new RavenJArray(answers);
        }
      }
    }
  }
}
```

The RavenJObject is an object that allows you access to a parsed JSON document. You can access and overwrite sections of your entity directly! After iterating through, save your results back to Raven and your data is migrated to the new form!

Suppose you needed to rename your variable and delete the old one? That's easy while you are manipulating the object this way already...

```
...
result.Remove("Answer")
result["Answers"] = new RavenJArray(answers);
...
```

Good luck migrating!
