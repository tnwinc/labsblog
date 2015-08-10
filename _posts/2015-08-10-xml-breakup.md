---
published: false
---

## Breaking up (an XML File) is hard to do

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
      $xmlTarget.WriteAttributeString('xmlns','tnw:grc:import:people')

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




