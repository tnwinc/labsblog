---
title: "NetApp & The Powershell Toolkit – Building More Intelligent Aggregate Selection"
layout: post
tags: [powershell, netapp, SAN, storage, infrastructure, sysadmin]
published: true
author: Matthew Fugel
mail: "matthewfugel@tnwinc.com"
summary: The easy way, vs the right way. Building a more bullet-proof method of selecting aggregates on NetApp, via Powershell
---
_My name is Matthew, and I'm a systems engineer on the IT Network Operations team; one of my day-to-day roles is storage administration. This post covers a sub-set of a Powershell script I was working on to make my life easier, but I found the challenge and the process of discovery to be worth sharing. You can find more of my writings [at my blog, here.](http://matthewfugel.wordpress.com)_

[![Give me all the disks of a certain flavor](https://matthewfugel.files.wordpress.com/2015/09/20150908-diskswhereaggregate.jpg?w=660)](https://matthewfugel.files.wordpress.com/2015/09/20150908-diskswhereaggregate.jpg)

I will admit that I am new to Powershell, but realize its value and significance in the future -- after all, Windows Server Core has arrived, environments are growing in breadth and number, and there will be a tipping point where some degree of automation is going to be the only way a sysadmin can keep up.

To that end, I've been poking at the NetApp Powershell Toolkit for some time. I've been using modified sample scripts to do things like create scheduled Flexclones of volumes containing SQL databases (and other no-SQL DB volumes) for Disaster Recovery or testing purposes. Now, with the help of my [DevOps teammate ](http://robertlabrie.wordpress.com)I am attempting to leverage some Powershell voodoo to create more exciting home-grown scripts. 

We recently decided that, moving forward, we would begin to leverage NFS as our format of choice, rather than VMFS, for VMWare datastores. The advantages of NFS made it a clear choice -- let's pin this for a future post. 

Because of the automation DevOps is cooking up to automatically stand up new servers, and stand them up in the right place, a large number of new datastores were requested. Creating volumes, creating NFS shares, applying NFS share permissions to ESX hosts, and mounting the NFS share to each host in the appropriate environment isn't terribly strenuous work, but it is a large number of steps. This isn't something we do every day, either --- but repeat this same task 10x, and there's a decent bit of work. Let's automate it! 

My intention for creating a scripted process for the above is to still have it attended --- but feed it a few values at the prompts, and it goes on its way doing your bidding. I recently cabled in a monstrous new NetApp 24x4TB TDE SATA shelf, mostly for housing SQL backups so we can stop parking them on expensive SAS, but DevOps is interested in running some VMs on it too -- most of the data they'd be manipulating will get loaded into VM memory anyway, with very little going to disk, so why not.

Okay, so this could be simple enough to script out --- prompt the user for which NetApp aggregate they want, then specify that aggregate when creating a new volume. But let's make it friendlier --- prompt the user basically for, _"would you like SAS or SATA?". _Determining the answer, in my environment, is easy enough, because I have aggregates named like this:

[![All my children](https://matthewfugel.files.wordpress.com/2015/09/20150908-aggregates_all.jpg)](https://matthewfugel.files.wordpress.com/2015/09/20150908-aggregates_all.jpg)

So you _could_ simply say
`Get-NaAggr | Where-Object {$_.Name -like "*SATA*"}` 
But, that's scripting around an environment I know, with a very descriptive naming convention. What it someone in another environment had less descriptive aggregate names? 

If you look at the output of `get-NaAggr `you do _not see_ the types of disk contained in that aggregate. If you go look at NetApp OnCommand System Manager, you'll see the same. OCSM does not associate a disk of a certain type with an aggregate, because presumably you could have an aggregate with mixed disk types in it -- say, SSD and SAS. 

Digging around in OCSM, you'll see that if you click into 'Disks', you'll see an attribute for each disk, identifying the _aggregate to which it belongs**. **_**Okay great!** Now let's discover that same attribute through Powershell, using an example disk. 

`get-nadisk -Name 0b.01.1 | format-list` 

[![Disk Properties](https://matthewfugel.files.wordpress.com/2015/09/20150908-effectivedisktype.jpg)](https://matthewfugel.files.wordpress.com/2015/09/20150908-effectivedisktype.jpg)<span style="line-height: 1.7;">

You can see from the output of the above command, the disk is telling us the aggregate it belongs to. But there is also a property returned called `EffectiveDiskType`. Great!

> _You'll see there are actually two properties for disks indicating their type; **DiskType** and **EffectiveDiskType.** Digging into this further revealed a NetApp Support doc,_ [How you can use effective Data ONTAP disk type for mixing HDDs:]
(https://library.netapp.com/ecmdocs/ECMP1203768/html/html/GUID-149D31D4-D0F5-47C6-A19E-E77B8A12C971.html)

> _`Certain Data ONTAP disk types are considered equivalent for the purposes of creating and adding to aggregates, and managing spares. Data ONTAP assigns an effective disk type for each disk type. You can mix HDDs with the same effective disk type.`_ 

> _As the table in this doc indicates, although there are many different varieties of disk supported in the NetApp, they all **effectively boil down** to being either **SAS** or **SATA.**_

So, what we'll have to do in Powershell is:

1.  Prompt the user, "SAS or SATA"? -> Store to a parameter
2.  Take the user input and go query all the disks for their property `EffectiveDiskType`
3.  For disks whose type matches the user's input, then query for the aggregate to which they belong.
4.  Return that aggregate as the targeted aggregate.

So let's work on this and start filtering down to something usable.

`get-NAdisk | where-object {$_.EffectiveDiskType -like "*SATA*"}` 

[![Give me all the disks of a certain flavor](https://matthewfugel.files.wordpress.com/2015/09/20150908-diskswhereaggregate.jpg?w=300)](https://matthewfugel.files.wordpress.com/2015/09/20150908-diskswhereaggregate.jpg)

So there are all my SATA-flavored disks, with their normal additional properties as displayed via `get-NaDisk`. Distilling our results down further, let's return only the property `Aggregate:` 

`get-NAdisk | where-object {$_.EffectiveDiskType -like "*SATA*"}  | Select Aggregate` 

[![Refining our results](https://matthewfugel.files.wordpress.com/2015/09/20150908-diskswhereaggregate-2.jpg?w=300)](https://matthewfugel.files.wordpress.com/2015/09/20150908-diskswhereaggregate-2.jpg)

OK, now let's filter out all the duplicates using `Get-Unique:` 

[![20150908 diskswhereaggregate 3](https://matthewfugel.files.wordpress.com/2015/09/20150908-diskswhereaggregate-3.jpg?w=660)](https://matthewfugel.files.wordpress.com/2015/09/20150908-diskswhereaggregate-3.jpg)

Cool, now let's trash that header, so we can pass our value into a variable for safekeeping: 

`(get-NAdisk | where-object {$_.EffectiveDiskType -like "*SATA*"}  | Select Aggregate | Get-Unique).Aggregate` 

[![Ding!](https://matthewfugel.files.wordpress.com/2015/09/20150908-diskswhereaggregate-4.jpg)](https://matthewfugel.files.wordpress.com/2015/09/20150908-diskswhereaggregate-4.jpg)

And there it is! OK, so one step further... testing using "SAS" instead... my SATA disks are owned by the opposite controller from the one I'm connected to, which means the aggregate name is not returned. To correct, a slight tweak of the command, which ultimately gives us **this command**: 

`$aggr= (get-NAdisk | where-object {($_.EffectiveDiskType -like "*SATA*") -and $_.Aggregate}  | Select Aggregate,Node | Get-Unique).Aggregate` 

So we could have targeted our aggregate by taking the easy route, just looking for a match in the name, since we already know we have good naming conventions... But isn't it so much more elegant, practical, and _versatile_ to say, 'Go figure out what _kind_ of disks we have available, and give me the name of the aggregate containing them'. You could run this against _any_ NetApp, and find what you're looking for!

> Unless you're Clustered-DataOnTAP -- then it's `get-NcDisk` -- right?  :D
