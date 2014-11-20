---
layout: post
title: The complicated world of splice QTLs
published: true
tags: science
categories: science
date: 2014-11-20 00:00:00
---

**Summary**: There is a lot of interest in analyzing the effect of genetic variants on splicing, but there is no consensus on the best way to analyze "splice QTLs". A variety of methods have been used, each answering slightly different questions.

With the new widespread availability of large scale RNA-sequencing datasets (e.g. from the [gEUVADIS](link here) and [GTEx](link here) projects) from individuals that also have genome-wide variation data, we can now start to identify genetic variants involved in regulating not just gene expression level, but also in regulating splicing events. There have been a lot of studies in the last several years surveying genetic effects on transcriptomic variation (e.g. [Lappalainen et al. 2013](link here), [Pickrell et al. 2010](link here) [Montgomery et al. 2010](link here), [Battle et al. 2013](link here), [Zhao et al. 2013](link here), [Monlong et al. 2014](link here)), but there is no consensus on the best way to analyze "splice QTLs" (sQTLs).

For one thing, while several good tools exist, it is generally agreed that accurately quantifying expression levels of different isoforms is not a solved problem. But another challenge is that unlike gene expression levels, which can be for the most part represented by a single value per gene, there is a confusing variety of ways one might represent the phenotype of an alternatively spliced gene. Each of these representations is best suited to identify differnent types of events, and each comes with its own set of advantages and disadvantages. 

So what is the best method to analyze sQTLs? What do most splicing variation events look like? (single exon events, or are they more complex?) How does the sQTL method used affect which events are likely to be picked up? These are some of the questions I have as a relative newcomer to the RNA-seq/splicing world, and from what I can tell we still don't know the answer to these questions.

Below I summarize the methods I've seen used in some recent papers and what I think are advantages and disadvantages of each. In my opinion there is no "best method", and the method one chooses is strongly dependent on the type of events you're looking for or expect to find.

## Methods to detect sQTLs
Each method for detecting QTLs has the same general outline: define a phenotype, test for association between that phenotype and the genotype of a nearby variant. For gene-level eQTLs, this is usually straightforward: the phenotype is the expression level of the gene (after some normalization, removing covariates, etc.), and the genotype is something like 0/1/2 for a biallelic variant. For analyzing sQTLs, the question is *how do you define the phenotype to capture splicing variation?*. Each method below does this slightly differently.

### Exon expression level
One widely used phenotype is simply exon expression level. (i.e., for each exon, how many reads mapped to that exon, after normalizing for things like the length of the exon, not going to get into FPKM/RPKM/TPM here).

TODO: picture here

Then you would simply test for association between expression of each exon and each variant nearby. This is the method used by [Montgomery et al.](link here) and one of the methods used in [the gEUVADIS Project paper](link here). 
* **Advantages**: This approach is useful because it captures variation both at the gene level (classical eQTLs), and variation in splicing level by looking at individual exons separately. For instance, the gEUVADIS paper claims that as many as 34% of genes with an exon-QTL have a second independent signal for another exon, suggesting some independence of exons in the same gene representing splicing variability.
* **Disadvantages**: This method does not give information about transcript structure in particular since each exon is analyzed independently, and so does not allow direct influence of the types of events leading to isoform variability. Also, this method involves doing a lot of non-independent tests, since many exons from the same gene will have correlated expression levels.
* **How many are there?**: gEUVADIS finds 7,825 genes with an exon QTL, compared to only 3,773 genes with a classical gene level eQTL at 5% FDR, suggsting that genetic regulation of splicing variation is more widespread than just variation in expression levels. 

### Transcript ratio
Another commonly used phenotype is transcript ratio: for each isoform, what percent of the total transcripts from that gene come from that isoform?

TODO: picture

Then you test for association between ratio for each transcript and each variant nearby. This method is used in [the gEUVADIS Project paper](link here) and by [Battle et al.](link here). [Montgomery et al.](link here) look at transcript expression levels (yet another method to represent phenotype), but do not look at transcript ratios.
* **Advantages**: This method captures information about use of different transcripts, and can be used to discern what alternative splicing events tend to look like (skipping a single exon? alternative exon usage? more complex events?). Additionally, unlike exon expression levels, it is specific to splicing events (i.e. it will not pick up gene level eQTLs).
* **Disadvantages**: It usually relies on known transcript annotations (although one could use this method including novel splice isoforms detected in their data). Therefore, it could be that there is for instance a simple exon skipping event, but if the two different isoforms (one with that exon, one without) aren't annotated, it could never be picked up. Using known transcript annotations already pre-defines the set of events you can hope to detect. Additionally, it relies on transcript level quantifications which are still not widely trusted. Finally, like the exon expression level method, this method involves many non-independent tests, since transcript ratios will be correlated for transcripts from the same gene (they must sum to 1).
* **How many?**: gEUVADIS identifies 639 genes with "transcript ratio QTLs" at 5% FDR. From the exon level analysis mentioned above, where there were several thousand more exon QTLs than gene QTLs, I would have thought at least several thousand genes show splicing variation. So clearly the transcript ratio method isn't picking up every event. This could possibly be because: it has reduced power since it's based on noisier transcript quantifications, or because known transcript annotations don't capture a large fraction of true splicing events.

### Percent exon inclusion (or PSI=percent spliced in)

This method is concerned with inclusion rates of single exons at a time: how often is a given exon included vs. excluded from a transcript?

TODO: picture

Then you test for association between the PSI of each exon and each nearby variant. Really this is asking: of isoforms that could contain this exon, what percent of them do? This method is used in [Monlong et al](link here) to compare to a different method they developed (see below). By design, it specifically captures exon skipping events, but not other types of variation in transcript structure. PSI is mentioned in [the gEUVADIS Project paper](link here), although they do not perform QTL analysis on these values.

A variation on exon inclusion is used in [Pickrell et al](link here), where they use as a phenotype the fraction of reads mapped to each exon divided by all reads mapped to that gene. This captures a different set of events than "PSI": for instance it would capture events involving switching between different transcripts that could differ by multiple exons, where as PSI is very specific in that it captures only events that differ by exclusion specifically of a single exon.

* **Advantages**:
* **Disadvantages**:
* **How many?**:

### sqtlseeker
transcript level


## What do most splicing events look like?
- overlap between methods
- what do they look like