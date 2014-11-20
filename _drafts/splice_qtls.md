---
layout: post
title: The complicated world of splice QTLs
published: true
tags: science
categories: science
date: 2014-11-20 00:00:00
---

**Summary**: There is a lot of interest in analyzing the effect of genetic variants on splicing, but there is no consensus on the best way to analyze "splice QTLs". A variety of methods have been used, each answering slightly different questions.

With the new widespread availability of large scale RNA-sequencing datasets (e.g. from the [gEUVADIS](link here) and [GTEx](link here) projects) from individuals that also have genome-wide variation data, we can now start to identify genetic variants involved in regulating not just gene expression level, but also in regulating splicing events. There have been a lot of studies in the last several years surveying genetic effects on transcriptomic variation (e.g. [Lappalainen et al. 2013](link here), [Pickrell et al. 2010](link here) [Montgomery et al. 2010](link here), [Battle et al. 2013](link here), [Zhao et al. 2013](link here)), but there is no consensus on the best way to analyze "splice QTLs" (sQTLs).

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
* **Disadvantages**: This method does not give information about transcript structure in particular since each exon is analyzed independently, and so does not allow direct influence of the types of events leading to isoform variability.
* **How many?**: gEUVADIS finds 7,825 genes with an exon QTL, compared to only 3,773 genes with a classical gene level eQTL at 5% FDR, suggsting that genetic regulation of splicing variation is more widespread than just variation in expression levels.

### Transcript ratio
Another commonly used phenotype is transcript ratio: for each isoform, what percent of the total transcripts from that gene come from that isoform?

TODO: picture

This method is used in [the gEUVADIS Project paper](link here) and by [Battle et al.](link here).
* **Advantages**: 

### PSI

### Other methods
transcript level


## What do most splicing events look like?
- overlap between methods
- what do they look like