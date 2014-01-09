---
layout: post
title: Show me the data
published: true
tags: science data
categories: science
date: 2014-01-9 00:19:00
---

As a PhD student in bioinformatics working with a lot of large datasets from diverse sources, I have built up a good list of things that annoy me about how scientific results and data are distributed and presented. In general, I'm surprised at how much we bury and obscure data deep inside of Word and PDF documents, and how little emphasis is put on transparency, reproducibility, and accessibility of datasets and methods used in publications. As a result, we spend a lot of time redoing analyses and not being able to reproduce results. This is not the case in every field: for instance, statisticians may release an R package with their manuscript that allows other scientists to completely reproduce the entire study. There has already been a lot of discussion on this (e.g. see the very relevant post [beyond-the-pdf-is-epub](http://blogs.plos.org/mfenner/2011/01/23/beyond-the-pdf-is-epub/), [this discussion](https://github.com/swcarpentry/bc/issues/199) and [this Nature News article from last year](http://www.nature.com/nature/journal/v482/n7386/full/nature10836.html)), but we have a long way to go.

Many people and organizations are thinking hard about these problems. For instance, [eLife](http://elife.elifesciences.org/) hopes to take advantage of its "being a digital-born publication" to help get the ball rolling in transforming how science is disseminated. They are trying to avoid publishing supplementary PDFs. In addition, all objects in an article, such as figures and tables, get their own DOI and a special link. I think this is an awesome step in the right direction. On another front, platforms like [Arvados]((https://arvados.org/) hope to offer bioinformaticians tools to make analyses completely transparent, from the raw data up to the final result. Tools like [IPython notebook](http://ipython.org/notebook.html) for Python and [knitr](http://yihui.name/knitr/?utm_source=dlvr.it&utm_medium=facebook) for R are making it easy to generate documents that integrate text, code, and figures, in a seamless and reproducible way. These are just examples of many efforts to start solving these problems. 

I tried to think about what I would envision as the ideal publication format, and listed some of the key points below.

## Five things that would make everyone happier

### 1. Provide the source code
Better yet, put all the code for the project in a repository on Github and document in the methods which revision number you used for each analysis.

I cringe when I read things like "we used a custom perl script to..." or "we used the standard pipeline with tool X", who knows with which version and parameters. Simple solution: **take the time to set up version control, then supply the revision number of each script you used for the analysis**. This is one of my favorite parts of what [Arvados](https://arvados.org/) is doing: when you develop and run a pipeline, you can trace back for each output file exactly what code versions and data files were used to create it. (Hmm... did I run that script before or after the bug fix?) This gives absolute transparency in how all the analysis was performed. You can hear how excited I am about this [here](https://arvados.org/blogs/11), in a talk I gave at the Arvados Summit.

Even better yet, make your code reusable by others. That could mean writing nice, documented, command line interfaces so other people can run the same scripts on their own data. Or it could mean writing a library packaging the new methods you had to create. It takes more time, but in the long run it will:

* Save scientists from constantly repeating analyses that others have already done.
* Make results more reproducible
* Make everyone write cleaner and probably more correct code
* Increase the impact of your work because it will be more directly accessible to other scientists.
* Make the work more credible since everyone can see exactly what was done.

There are certain analyses where this won't work. Here are a couple examples and partial solutions:

* If the analysis was done in Excel, provide the Excel file with the data and commands you used.
* If an external web application was used, provide the link to the results and which parameters you used.

### 2. Link figures and tables to the code and data used to generate them

Wouldn't it be great if you were reading an article, wanted to see how they generated a plot, clicked on the figure, and magically the source code appeared? Many times the one sentence legend or the vague Methods section is not enough for you to know exactly how something was done. If you have full access to the code, there is no room for ambiguity. Even if you use tools like Adobe Illustrator to edit the raw figures, you can still provide a link to the source code and the raw output before modification. Same thing goes for tables. Most tables are data and code driven. By linking the code to the table, readers can know *_exactly_* how those numbers were generated. Often times a table will be truncated due to space (e.g. Table X gives the top 10 Y). In these cases, link the table to the complete dataset from which it was made.

Providing code for figures and tables promotes reproducibility, but [it is also educational](http://www.nature.com/nature/journal/v482/n7386/full/nature10836.html) and can save other people from wasting a lot of unnecessary time. Maybe I read a paper and I really like a figure there. I'd like to make something similar for my own manuscript. Right now, this would usually mean spending a lot of time of frustrated googling trying to figure out how to make the plot. If the source code is there, others can modify it to generate their own plot (while of course citing you in the process).

### 3. Link results cited in the text to the code used to generate them

Common scenario: I read. "We found that the average score of X on Y (after doing 8 million rounds of sketchy filtering steps) is Z." I then proceed to the supplemental to download the raw data, try to recreate my interpretation of the steps they took, and end up with a (hopefully only slightly) different result.

Solution: **for each quantitative result that is reported, include a link to the source code for the steps to generate that result**. If a reader wants to see where that number came from, they follow the link on the number to pop up the relevant commands.

### 4. No more PDFs

As has been pointed out ([here](http://www.gizmodo.co.uk/2012/07/microsoft-pdf-is-where-documents-go-to-die/) and by many others), **PDFs are where data goes to die**. Good luck trying to analyze the data in that 10-page table from the supplemental material. You could try copying and pasting into Excel while crossing your fingers. If you really want that data it's gonna cost you hours of manual work. Probably most data from papers that I actually end up using is in the supplemental, making this problem especially annoying. We can think of much better ways to do this.

One Solution: **make the supplementary material an interactive HTML**. How much fun would it be if supplementary material was presented in the form of [IPython notebooks](http://ipython.org/notebook.html)? I think that is the goal we should aim toward. Maybe not IPython notebooks per se (although I would certainly be all for that), but in some digital, interactive format. This would ideally take into account all of the issues mentioned above: all figures, tables, and results will be linked to the data sources and code used to create them.

### 5. Make ALL the raw and processed data accessible

This last one's a lot harder, but it's hugely valuable.

What are all the SNPs that have been associated with this trait? What are all the predicted eQTLs for this gene? Does that transcription factor bind to my favorite region of the genome? These are examples of common questions researchers might have, but they're never going to find the answer for their specific gene or trait or property X of interest from just reading the paper or even the supplementary material. If they're lucky, the paper provides enough raw data that the answer is hiding in there somewhere after enough processing. But many times these answers are not easy to dig out of the raw data. And probably a lot of these answers had to be computed along the way. The end result is that a lot of time gets wasted by people munging through analyses that the authors of the paper already did.

To maximize the usefulness of data, users should be able to interact with it. *Even the data that was used for intermediate results and didn't get displayed.* Ideally, papers generating large datasets would offer a tool, such as a web portal, making these results easily accessible and searchable by other scientists. If relevant, one nice option is to make data available as a track for the [IGV](http://www.broadinstitute.org/igv/) or [UCSC Genome Browser](http://genome.ucsc.edu/) . 

A lot of consortia are already doing this: many datasets can be accessed through the UCSC Genome Browser. Many are setting up portals (e.g. [gEUVADIS](http://www.ebi.ac.uk/Tools/geuvadis-das/), [GTeX](http://www.broadinstitute.org/gtex/), and [1000 Genomes Project](http://browser.1000genomes.org/index.html)). Some individual labs/projects have set up similar portals, but for the majority of data used in publications this is not the case.

This is not trivial: who has time to program a web application, or money to hire someone to do it. Still, in the long run making the data not only *available* but also *accessible* will be a huge leap forward.

## Conclusion

This all sounds great, but it's far from the current reality. I cannot claim I have done all of the above, but I will work and am working hard to implement as much of these points as possible the next time around. It's a long way to go before we get to a world where all results are maximally useful to other scientists. A lot of groups are pushing things in the right direction, but it will be a community effort and mentality shift to put more emphasis on reproducibility and transparency rather than just on the results themselves. 