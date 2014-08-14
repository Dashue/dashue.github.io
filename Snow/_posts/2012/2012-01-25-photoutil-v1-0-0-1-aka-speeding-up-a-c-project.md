---
layout: post
title: PhotoUtil v1.0.0.1 aka Speeding up a c# project
categories: .Net, Opensource, Pet Project
published: true
---

---excerpt
My media organizing pet project was running a bit slow. This is how I was able to analyze, identify and solve the biggest bottleneck using visual studios performance wizard.
---end

I was investigating why my hobby project ["PhotoUtil"](https://github.com/Dashue/MediaOrganizer) was being slow.
A program that takes a folder with a bunch of photos as inputs, processes the metadata from them, more specifically the date the picture was taken and then organizes them in folders based on this.

Following is a short recap of that procedure.

##Find out what's taking time 1: Analyze / Launch Performance Wizard

The first thing that popped out at me was a DateTime creation which took 100% of the CPU time (according to the performance report). I grab the DateTimeOriginal value from exif metadata of a photo and convert it into datetime, only because it allowed for cleaner code when accessing the date and time values later on in the code. Apparently parsing strings into datetime is slow, so instead I got it to work with some string manipulation.

##Find out what's taking time 2: Analyze / Launch Performance Wizard

When reading exif data using GDI+ from a file the whole image is read into memory using new Bitmap(Path); then the desired properties are fetched using GetProperty. This made me switch focus to increasing concurrent files being worked on (read parallelization) instead of minimizing process time of each file. With this in mind I rewrote my foreach loop into using the parallel library.

from

	foreach(var filepath in filepaths)
into

	Parallel.ForEach(filePaths, oldFilePath); 
	

##Taking it one step further:

After finding out that the whole image is read into memory I set out to find an alternate approach of extracting EXIF data from a photo. It wasn't long before I found a sweet project called [ExifLib](http://www.codeproject.com/Articles/36342/ExifLib-A-Fast-Exif-Data-Extractor-for-NET-2-0) on [Nuget](https://www.nuget.org/packages/ExifLib/) which only reads the Exif data portion of an image. ExifLib is a wonderful lib created by Simon McKenzie only available as source code from codeproject.com but I'm hoping to convince him to put it up on NuGet so more people can find and make use of it.
 
##Results
100 images processed on an Intel Core 2 duo

	Before optimizations:	00:00:20.8059121  
	After optimizations:	00:00:11:4034511
	Using ExifLib:			00:00:01.0714248