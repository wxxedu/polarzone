---
title: "PPT to PDF: Batch Conversion Using AppleScript"
date: 2022-02-05T15:04:53+08:00
draft: false
series: ["Random Snippets"]
tags: ["Mac Automation", "AppleScript", "DevonThink 3"]
---

> If you are just here looking for the solution, this is my [github link](https://github.com/irmowan/Convert-ppt-to-pdf/blob/v1.0/Convert-ppt-to-pdf.applescript) for the script. It should be pretty straight-forward. 

I am a [DevonThink](https://www.devontechnologies.com/apps/devonthink?pk_campaign=google&pk_kwd=devonthink1&gclid=Cj0KCQiAuvOPBhDXARIsAKzLQ8GQyXWN2dA_TXJXdUaphju1dXhEpFfPkciONc0OCPxDEm_U7afd6s0aAjLMEALw_wcB) user. The thing that I love most about it is that it combines document organization tools with document viewing / editing tools. In one window, you will be able to edit the contents of a specific file while seeing the other files under the same directory.

![DevonThink Screenshot](/static/Random-Snippets/ppt-to-pdf/devonthink-screenshot-01.png)

It also offers many great tools, especially with batch processing. 

![DevonThink Batch Processing](/static/Random-Snippets/ppt-to-pdf/devonthink-batch-processing.png)

The sad thing is that this does not support PPTX files. As a college student, a lot of the lecture slides that I receive are in PPT format, and it would be just so annoying if I had to convert them one by one manually. As the saying goes,

> It's always better spending 10 hours figuring out how to automate a task than spending 5 minutes doing the task manually. 

So, natually, I started looking on line for solutions. Many thanks to [irmowan](https://github.com/irmowan/Convert-ppt-to-pdf), I was able to find out some code that supposedly converts PPT to PDF by using AppleScript and PowerPoint itself. The issue is though, the code that he posted on his github no longer worked. 

You can go check out his code via [this link](https://github.com/irmowan/Convert-ppt-to-pdf/blob/master/Convert-ppt-to-pdf.applescript#L6). When I ran the script, it would correctly open Powerpoint, but does not do the exporting correctly. 

One thing that I noticed first was that PPT was not closed. 

```applescript
tell application "Microsoft PowerPoint"
	quit
end tell
```

This means that some of the bugs must have occured before the closing statement above such that the closing statement would not be reached. To explore things better, I decided to see if the `active presentation` was the correct one, if there was one at all. 

so, I tested this by adding the code 

```applescript
return the name of active presentation
-- I don't know how to log things in AppleScript, 
-- so I just returned the value that I want to test
```

above the line 

```applescript
save active presentation in pdfPath as (save as PDF)
```

which gives me `missing value` as a result. This led me into thinking that when I was calling the save statement, the presentation may not be ready yet. Hence, I added a while loop before the lines
```applescript
repeat while active presentation is missing value
    delay 0.1
end repeat
```

this way, if the active presentation is missing, it will just keep repeating until the end of the universe (which probably isn't a wise thing, but who cares).

Surprisingly, that did work, and now I have the name of the presentation file. The only issue is that it still does not save the PDF file. 

Initially, I thought it was the problem with the statement `(save as PDF)` in the saving statement 

```applescript
save active presentation in pdfPath as (save as PDF)
```

but after checking the documentation and trying out all kinds of different combinations, I could not get it to work. Then, I decided to delete the `in pdfPath` part:

```applescript
save active presentation as (save as PDF)
```

amazingly, the exporting worked, just that nobody knows where the PDF file was saved to. I then went on to check the documentation for `save in`, and found out that the variable after `in` should be of type `file` rather than `string`. I looked up the examples on the `file` type, and found out that you can convert the path string to file using the statement `POSIX file filePath`. So, after the modifications, the code now looks like this:

```applescript
save active presentation as (save as PDF) in POSIX file filePath
```

and I could batch convert the PPTs to PDFs. Hurray!

Here is the full code:

```applescript
on PPToPDF(input)
	set theOutput to {}
	tell application "Microsoft PowerPoint" -- work on version 15.15 or newer
		launch
		repeat with i in input
			set t to i as string
			if t ends with ".ppt" or t ends with ".pptx" then
				set pdfPath to my makeNewPath(i)
				open i
				repeat while active presentation is missing value
					delay 0.1
				end repeat
				save active presentation as (save as PDF) in POSIX file pdfPath
				set the end of theOutput to pdfPath
			end if
		end repeat
	end tell
	tell application "Microsoft PowerPoint" -- work on version 15.15 or newer
		quit
	end tell
	return theOutput
end PPToPDF

on makeNewPath(f)
	set t to f as string
	if t ends with ".pptx" then
		return (text 1 thru -5 of t) & "pdf"
	else
		return (text 1 thru -4 of t) & "pdf"
	end if
end makeNewPath

-- Ignore the part below if you do not use DevonThink!

tell application "DEVONthink 3"
	set theSelection to the selection
	if theSelection is {} then error "Please select a record"
	set filepath to the path of the first item of theSelection
	set filePaths to {}
	repeat with fileItem in theSelection
		set end of filePaths to the path of fileItem
	end repeat
	set theOutput to my PPToPDF(filePaths)
	return theOutput
end tell
```