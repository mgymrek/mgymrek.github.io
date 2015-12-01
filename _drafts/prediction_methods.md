---
layout: post
title: The death of QTLs? Exciting new methods for predicting regulatory function
published: true
tags: science
categories: science
date: 2015-12-03 00:00:00
---

**Summary**: New methods that use machine learning techniques to directly predict regulatory properties of non-coding sequence will likely play a key role in interpreting non-coding genetic variation. 

Understanding the impact of non-coding genetic variation is key to understanding heritable complex traits in humans. The vast majority of loci (~95%) identified by genome-wide association studies (GWAS) implicate non-coding variation. This suggests that variation in regulatory function, rather than in protein coding sequences, is largely responsible for common diseases like diabetes and Crohn's Disease. One common hypothesis is that many of these loci share an underlying model: a genetic variant affects binding of some factor to DNA, which affects transcription of a nearby gene(s), which affects some downstream cellular process leading to disease (even though we're [learning more and more that transcriptional changes might not necessarily lead to actual changes in protein levels](https://speakerdeck.com/stevemunger) (shoutout to [@stevemunger](https://twitter.com/stevemunger) who gave a great talk on this at ASHG). It sounds simple, but interpreting non-coding variation turns out to be extremely challenging.

## Learning the regulatory code
So how can we predict which non-coding variants have "causal" effects leading to disease? Here are some options:

* **Is the variant conserved across evolution?** Then it might be important. But this doesn't tell us a whole lot about what that variant might be doing and what cells it's doing something in. Plus it will miss things that are only important in humans.
* **Does the variant overlap some annotation predictive of regulatory activity?** E.g., does it overlap DNAseI hypersensitive sites, certain histone modifications, or certain transcription factor binding sites? With projects like [ENCODE](https://www.encodeproject.org/) and the [Epigenome Roadmap](http://www.roadmapepigenomics.org/), we now have tons of cell-type specific regulatory annotations. But just because a variant overlaps (or doesn't) one of these annotations doesn't mean it has any causal relationship with that annotation.
* **Is the variant a QTL?** i.e., is the genotype of that variant associated with varying levels of your annotation of interest? Detecting QTLs requires measuring your phenotype of interest in dozens to thousands of samples. And at the end of the day, you still are left with a bunch of associations, which say nothing about causality.
* **Is the variant *directly predicted to modulate a phenotype of interest* in a given cell type?** Until recently, we didn't have a good way to answer this question besides performing months or years of experimental work. But new machine learning methods are giving us ways to start producing meaningful predictions of non-coding function.

# New methods provide direct sequence-based predictions of regulatory activity
In the last several months, a handful of new methods have come out for predicting regulatoy activity of non-coding regions. All of these take similar forms: train a machine learning model to predict an annotation of interest based on local genome features. These models seem to mostly capture features related to sequence specificities of transcription factors, but they can also take into account things like broader sequence context, co-binding of different factors, etc. There are several things I find beautiful about these methods. First, we can now use these models to directly predict the impact of a mutation by feeding the model different versions of sequences containing different alleles and looking at the difference. Second, since very accurate models can be built using a single dataset from a cell type of interest, these methods preclude the need to measure these molecular phenotypes across hundreds of samples as is required for QTL analysis. This is a *huge* advantage. Below I take a brief look at some recently published methods (sorry if I am missing some), divided into two general classes.

### Kmer-based models

One class of methods are "kmer" based, meaning they train an underlying model to learn the effect of short kmers (e.g. <=10bp) on local sequence annotations. e.g. every time I see the kmer ATCG I see tons of my transcription factor binding but every time I see AAAT I see no binding.

* **deltaSVM** TODO
* **GERV** TODO

### "Deep learning" using convolutional neural nets
A second class of methods relies on "deep learning" (side note, every time I hear that word I can't help but thinking about [this tweet](https://twitter.com/dgmacarthur/status/654922513009405952) by Daniel MacArthur). Specifically, these methods rely on a technique called deep convolutaional neural networks (CNNs), which I had admittedly never heard of until a couple months ago, but I should probably learn more about. CNNs can capture complicated nonlinear sequence features that might not be well captured by kmer based approaches.

* **DeepBind** TODO
(mention new company)
* **deepSEA** TODO
* **Basset** TODO

### Are these methods accessible to non-machine learning gurus?
I was happy to see that all of these studies made some or all of their methods available by providing the source code, interactive web applications, or both. deltaSVM provides source code and precomputed models. GERV comes packaged in a [Docker](www.docker.com) container, and has extensive instructions on how to use it on the Amazon cloud. DeepSea provides a webapp to perform in silico mutagenesis or predict the epigenetic state of a sequence as well as the source code to do so. DeepBind has a webapp to visualize motif patterns discovered by their models plus packaged binaries to perform predictions, although I didn't see a way to train new models. Finally, Basset lives up to its claim of making all of this accessible. Although it took a little effort to install all the dependencies, there is good documentation and wonderful IPython tutorials walking through many of the steps to train and apply models. Kudos to all of these studies for making these things accessible.

# What can we do with these?
I think these methods have huge potential applications in human genetics. One criticism I've heard is that these just provide one more annotation to add to our list. But they are much more than that: we can now take a given variant of interest and directly predict its impact in a specific cell type  after doing only a single experiment. I think that will prove to be a valuable tool going forward as we begin to sift through all of these non-coding variants in much deeper ways than we've been able to do with only genetic associations.
