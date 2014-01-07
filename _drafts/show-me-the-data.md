---
layout: post
title: Show me the data
published: false
tags: science data
categories: science
---

# Intro
- transparency, provenance, big data
- working a lot with big data, built up a good list of things that annoy me, thinking about things that would make my life easier
- got involved in arvados. lot of ppl thinking about these problems (altshuler, genome bridge, others?)
- also journals looking at how to change things (elife)

# Five things that would make us all happier

## 1. Provide the source code ##
Better yet, put all the code for the project in a repository on Github and document in the methods which revision number you used for each analysis.

I cringe when I read things like "we used a custom perl script to..." or "we used the standard pipeline with tool X", God only knows which version and parameters. Simple solution: take the half an hour to set up version control, then supply the revision number of each script you used for the analysis.

Better yet, make your code reusable by others. That could mean writing nice, documented, command line interfaces so other people can run the same scripts on their own data. Or it could mean writing a new library packaging the new methods you had to create. It takes more time, but in the long run it will:

* Save scientists from constantly repeating analyses that others have already done.
* Make results more reproducible
* Make us all write cleaner and probably more correct code
* Increase the impact of your work because it will be more directly accessible to other scientists.

Even if your analysis is not in the form of scripts or libraries, for instance if you did your analysis in Excel, well then at least provide the Excel file with the data and commands you used.

reference Arvados here

## 2. Link figures and tables to the code and data used to generate them ##

## 3. Link results cited in the text to the code used to generate them ##

## 4. No more PDFs: Make the supplementary material an interactive HTML ##

## 5. Make ALL the raw and processed data accessible ##

 - portals to interact with data: impossible for ppl, especially not
   super programmers, to access results