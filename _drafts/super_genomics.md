---
layout: post
title: Super-genomics
published: true
tags: science
categories: science
date: 2015-01-03 00:00:00
---

There has been a lot of talk recently about super-enhancers. [This perspective](http://www.nature.com.libproxy.mit.edu/ng/journal/v47/n1/full/ng.3167.html) by Pott and Lieb just came out in *Nature Genetics* trying to assess "What are super-enhancers?". While there have been several papers using several different definitions, the underlying concept is the same: super-enhancers are "defined by very high levels of activator binding or chromatin modification". These super regulatory elements have been shown to determine cell fate, control cell-type specific expression patterns, and to be enriched for GWAS hits.

I belive "super enhancers" exist and are important, in the sense that are certain key regulatory elements that are important for controlling cell identity. But are super-enhancers actually some kind of novel regulatory element? The authors of the perspective suggest not: *"Our opinion is that there is not yet strong evidence that super-enhancers are a novel paradigm in gene regulation and that use of the term in this context is not currently justified."* I agree with this: similar "super" trends (exponentially decreasing distributions of some signal) are ubiquitous in biology and don't necessarily represent novel functional elements.

Let's take a look at how super-enhancers are defined. Pott and Lieb break it down into three steps (see their [Figure 1](http://www.nature.com/ng/journal/v47/n1/fig_tab/ng.3167_F1.html)):
* **Step 1**: Identify enhancer locations. This is usually based on ChIP-seq data for cell-type transcription factors or other marks of enhancers (e.g. H3K27ac).
* **Step 2**: Cluster enhancers. e.g. if an enhancer is less than 12.5kb away from another enhancer, combine them.
* **Step 3**: Identify super-enhancers. This is done by ranking the signal for some signal for each enhancer (again, either a master regulator TF or another mark such as H3K27ac). This will look like some exponentially increasing distribution. Choose the inflection point of this curve as a cutoff, and call anything above that point a "super-enhancer".

As Pott and Lieb point out, there have been many variations on these steps, but the common element to all approaches is step 3: "the only defining feature of super-enhancers is an exceptionally high degree of enrichment of transcriptional activators or chromatin marks as determined by ChIP-seq, which is assessed in step 3".

However, if you take almost any ChIP-seq experiment, not necessarily enhancer related, you are almost guaranteed to see a similar distribution of signal in Step 3. Why? Most factors bind DNA along some quantitative continuum of strengths: there are likely a small number of sites that are strongly bound, lots of sites that are only very weakly bound, and sites that are somewhere in between. This also makes sense thermodynamically: the number of sites on DNA recognized by a specific factor decreases exponentially with the affinity of the site for the factor (e.g. see [Wunderlich and Mirny 2009](http://www.ncbi.nlm.nih.gov/pubmed/19815308)). [Mark Biggin](http://www.cell.com/developmental-cell/abstract/S1534-5807%2811%2900406-0) has a nice perspective in *Developmental Cell* discussing this phenomenon.

Above I focused on transcription factor binding, but similar continua of signals are observed in every ChIP-seq experiment I've looked at (which is admittedly not that many). I downloaded a couple of ENCODE datasets to convince myself. As expected, looking at the top 1,000 peaks of H3K27ac in GM12878, we see the expected exponential "super-enhancer" distribution:

![H3K27ac]({{ site.url }}/images/h3k27ac.png)

But here's what it looks like if we instead take H3K27me3, a mark of *repressive* chromatin:

![H3K27me3]({{ site.url }}/images/h3k27me3.png)

I term the peaks on the far right **super repressors**!
	
and here's H3K4me3, a mark often found at promoters:

![H3K4me3]({{ site.url }}/images/h3k4me3.png)

I term the far right regions **super promoters**!

My point is not that super-enhancers as they are defined don't exist, but that they simply reflect the fact that the signal for enhancer regions, like that of many other biological features, follows a continuous and exponentially declining distribution. There are a small number of very strong signals, and these are more likely to be functionally important, and a huge number of not so strong signals, only some of which are likely to be important. Sure, one can take things at the top of this distribution and call them "super", but that does not make them a novel type of regulatory element.


