---
title: "Breaking up (an XML File) is hard to do"
layout: post
tags: 
  - xml
published: true
author: Doug Lewis
mail: "douglewis@tnwinc.com"
summary: I learned a few things trying to break up a gigantic XML file
---

I recently had a need to break up a large XML file.  We were getting import records with hundreds of thousands of entries and loading a single XML file with that much data was grinding processes to a halt.  I decided to whip up some Powershell to chunk these large files into smaller ones so the processing could go faster and so folks could open them in text editors to look at them with crashing their computers.  I expected this to be an easy task, but it was actually fairly daunting.  After a lot of Googling, I wound up with some code that did what I wanted and I want to walk through it.

	Function Batch-XMLFile {
      Param(
          [parameter(Mandatory=$true)] [string] $inputFile,
          [parameter(Mandatory=$true)] [string] $outputDirectory,
          [parameter(Mandatory=$true)] [int] $batchSize
      )

      $actualFile = Get-Item $inputFile
      $inputFileName = $actualFile.Name
      $batchCount = 1
      $personCount = 0
      $batchProgress = $batchSize
  
      Write-Host "About to Read in the source-data..."
      $time = Measure-Command {
          $rawSource = gc $inputFile -Raw
          $xmlSource = [xml]$rawSource
          $rawSource = ""
      }
      Write-Host "Your data is in memory. That took $($time.Hours) hours, $($time.Minutes) minutes and $($time.Seconds) seconds."  
      
      Write-Host "Getting chunky with batch $batchCount..."
      $outputFileName = $outputDirectory + "\" + $inputFileName
      $outputFileName = $outputFileName.Replace(".xml",($batchCount.ToString() + ".xml"))
      $xmltarget = New-Object System.Xml.XmlTextWriter("$outputFileName",$null)
      $xmlTarget.Formatting = 'Indented'
      $xmlTarget.Indentation = 1
      $xmlTarget.IndentChar = "`t"
      $xmlTarget.WriteStartDocument()
      $xmlTarget.WriteStartElement("People")
      $xmlTarget.WriteAttributeString('xmlns','whatever:your:schema:is')

      foreach($thisPerson in $xmlSource.People.Person) {
          $personCount++
          if($personCount -gt $batchProgress) {
              #Close the current target doc
              $xmlTarget.WriteEndElement()
              $xmlTarget.WriteEndDocument()
              $xmlTarget.Flush()
              $xmlTarget.Close()
              #Create a new target doc
              $batchCount++
              $batchProgress += $batchSize
              Write-Host "Getting chunky with batch $batchCount..."
              $outputFileName = $outputDirectory + "\" + $inputFileName
              $outputFileName = $outputFileName.Replace(".xml",($batchCount.ToString() + ".xml"))
              $xmltarget = New-Object System.Xml.XmlTextWriter("$outputFileName",$null)
              $xmlTarget.Formatting = 'Indented'
              $xmlTarget.Indentation = 1
              $xmlTarget.IndentChar = "`t"
              $xmlTarget.WriteStartDocument()
              $xmlTarget.WriteStartElement("People")
              $xmlTarget.WriteAttributeString('xmlns','tnw:grc:import:people')
          }
          $thisPerson.WriteTo($xmlTarget)
      }

      $xmlTarget.WriteEndElement()
      $xmlTarget.WriteEndDocument()
      $xmlTarget.Flush()
      $xmlTarget.Close()
	}
	
There are a number of things going on here that are worth looking at.  The first is my call to Get-Content `$rawSource = gc $inputFile -Raw`.  That -Raw flag is important.  Get-Content is actually a pretty expensive operation under normal use.  It parses each line of a text file, builds a custom Powershell object for each string and then builds an array of the objects.  If you do that will a file that's millions of lines long it will crush most desktop CPUs.  My computer is no slouch and I still had to just kill the operation after it had been spinning for 90 minutes.  By adding the `-Raw` flag, Get-Content works totally differently.  It now just builds a single string of all the text in the file.  The XML conversion of that string is much faster.  I can run through the same file with that little tweak in less than a minute.
  Another thing I had to learn my way through is the interplay of the XMLTextWriter and XML objects.  Trying to build the outer nodes of a target XML file from the data in the XML objects proved really frustrating.  In the end, it's much easier to just let the XMLTextWriter add that stuff line by line.
  Finally, though the XMLTextWriter handles line-by-line stuff just fine, you have to let the XML object manage writing itself out as I did with `$thisPerson.WriteTo($xmlTarget)`.  I hope that if somebody else finds themselves needing to do the same thing, that this will help them out.
