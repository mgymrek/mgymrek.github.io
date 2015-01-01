---
layout: post
title: Super-genomics
published: true
tags: science
categories: science
date: 2015-01-03 00:00:00
---

There has been a lot of talk recently about super-enhancers. [This perspective](http://www.nature.com.libproxy.mit.edu/ng/journal/v47/n1/full/ng.3167.html) just came out in *Nature* trying to assess "What are super-enhancers?". While there have been several papers using several different definitions, the underlying concept is the same: super-enhancers are "defined by very high levels of activator binding or chromatin modification". These super regulatory elements have been shown to determine cell fate, control cell-type specific expression patterns, and to be enriched for GWAS hits.

I belive "super enhancers" exist and are important, in the sense that are are certain key regulatory elements that are important for controlling cell identity, etc. But are super-enhancers actually some kind of novel regulatory element? The authors of the perspective suggest not: *"Our opinion is that there is not yet strong evidence that super-enhancers are a novel paradigm in gene regulation and that use of the term in this context is not currently justified."* I agree with this: similar "super" trends (exponentially decreasing distributions of some signal) are ubiquitous in biology.

Let's take a look at how super-enhancers are defined. Pott and Lieb break it down into three steps:
* **Step 1**: Identify enhancer locations. This is usually based on ChIP-seq data for cell-type transcription factors or other marks of enhancers (e.g. H3K27ac).
* **Step 2**: Cluster enhancers. e.g. if an enhancer is less than 12.5kb away from another enhancer, combine them.
* **Step 3**: Identify super-enhancers. This is done by ranking the signal for some signal for each enhancer (again, either a master regulator TF or another mark such as H3K27ac). This will look like some exponentially increasing distribution. Choose the inflection point of this curve as a cutoff, and call anything above that point a "super-enhancer".

As Pott and Lieb point out, there have been many variations on these steps, but the common element to all approaches is step 3: "only defining feature of super-enhancers is an exceptionally high degree of enrichment of transcriptional activators or chromatin marks as determined by ChIP-seq, which is assessed in step 3".

However, if you take a ChIP-seq experiment of *anything*, not necessarily enhancer related, you are almost guaranteed to see a similar distribution of signal in Step 3. Why? Most factors bind DNA along some quantitative continuum of strengths: there are likely a small number of sites that are strongly bound, lots of sites that are only very weakly bound, and sites that are somewhere in between. This also makes sense thermodynamically: the number of sites on DNA recognized by a specific factor decreases exponentially with the affinity of the site for the factor (e.g. see [Wunderlich and Mirny 2009](http://www.ncbi.nlm.nih.gov/pubmed/19815308)). [Mark Biggin](http://www.cell.com/developmental-cell/abstract/S1534-5807%2811%2900406-0) has a nice perspective in *Developmental Cell* discussing this phenomenon.

Above I focused on transcription factor binding, but similar continua of signals are observed in almost any ChIp-seq experiment. I downloaded a couple of ENCODE datasets to convince myself. As expected, looking at the top 1,000 peaks of H3K27ac in GM12878, we see the expected exponential "super-enhancer" distribution:

![H3K27ac]({{ site.url }}/images/h3k27ac.png)

But here's what it looks like if we instead take H3K27me3, a mark of *repressive* chromatin:

![H3K27me3]({{ site.url }}/images/h3k27me3.png)

I term the peaks on the far right **super repressors**!
	
and here's H3K36me3, a mark usually found on transcribed regions:

![H3K36me3]({{ site.url }}/images/h3k36me3.png)

and here we have **super transcription**!



