---
title: "Unbounding Results in PowerShell"
layout: post
tags: [powershell, ravendb]
published: false
author: Wright Malone
mail: "wrightmalone@navexglobal.com"
summary: Sometimes you want to get all of the documents of a certain type, and there isn't a ton of information on that for powershell.
---

Unbounding Results with Powershell

While working with RavenDB, we’ve gotten a lot of use from PowerShell to make our lives a little easier. As we started working with larger collections, the need to page the retrieved data became apparent. Raven has a page size cap of 1024 with rest, and we generally found 500 to be the better practice to account for longer docs causing potential timeouts. That paging would tend to look something like this:
 
    $count = 0
		$pageSize = 500
		while($count -lt $docsCount){
			$restUri = "$ravenUrl/indexes/$queryIndex&pageSize=$pageSize&start=$count"
			$results = Invoke-RestMethod -Uri $restUri -Method Get

			#Do stuff with results
			$totalResults += $results.Results
			$count += $pageSize	
		}

Using paging in this way returns all results as expected, but as the number of queried documents increases, the time overhead from making multiple calls to the server increases with it. As a result of that extra time, we wanted to look into quicker ways of retrieving data, hopefully removing the need for paging completely.
In exploring options, [a blog post was found](https://ayende.com/blog/161249/ravendbs-querying-streaming-unbounded-results) showing how to utilize a stream with C# to get all documents from a collection. While there is plenty of literature available on using this method in C#, examples in PowerShell were fewer and far between. Attempting to reproduce a solution similar to the one above ended up yielding something like the following, and our hope is that it makes streaming unbounded results in the future a little easier:

		$restUri = "$ravenUrl/streams/query/Raven/DocumentsByEntityName?$query"
		$request = [System.Net.WebRequest]::Create($restUri)
		$response = $request.GetResponse()

		$reader = New-Object System.IO.StreamReader $response.GetResponseStream()

		while(!$reader.EndOfStream()){
		    $rawData = $reader.ReadLine() 
		    $parsedData = $jsonserial.DeserializeObject($rawData)
		}

		$response.Close()
 
So basically the stream option in Raven’s API is utilized to create a streamed web request, then that stream is read and parsed to yield the same result as the paging done above. 
To see what kind of performance increase there was using this method, we took a collection of 10000 documents and measured the time that it took to retrieve each object. Each method was run 5 times, and the average of those runs used to measure the relative performance. The average run time for paging was about 36 seconds, while the average for streaming was about 33 seconds:

[![results table](/screenshots/unbounding-results-ps)](/screenshots/unbounding-results-ps "results table")

The benefits of streaming this unbounded result set become pretty clear when working with large collections of data, but as this is more or less a first-pass solution it is likely that there will be more ways to improve on it as we work with streaming results more. One of the more obvious examples of this is using the ReadLine method on our stream reader. As it stands there will be issues that stem from gathering larger amounts of data, as the full returned result is stored in memory. 

At the end of all this, I’m not sure which method I’ll be using day to day. Paging still feels a little more comfortable and possibly simpler even in more complicated scripts, but retrieving the full dataset at once provides a slight performance increase (especially on the first run) and is pretty damn cool.
