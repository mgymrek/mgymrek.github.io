---
layout: post
title: Thoughts Xiong, et al. "The human splicing code reveals new insights into the genetic determinants of disease"
published: true
tags: science
categories: science
date: 2014-12-31 00:00:00
---

**Summary**: My thoughts on the recent paper by [Xiong et al. in Science this month](http://www.sciencemag.org/content/early/2014/12/17/science.1254806.full), "The human splicing code reveals new insights into the genetic determinants of disease". Overall I think this study is very promising in showing how we can build powerful model-based predictions of gene regulation using sequence features. 

## Summary of the study

Xiong et al. build a model that uses nearby sequence features to predict for a triplet of exons, the percent of transcripts for which the central exon is spliced in (PSI) for a specific tissue. Although building such a model has been attempted before (TODO refs), including a study by many of the authors on this paper (TODO ref), their method seems to include a more comprehensive set of features and outputs absolute PSI values per tissue. Their model captures effects such as those of known RNA binding proteins that act in *cis* as well as *trans* acting factors, and shows that the effect of different sequence features depends both on the *cis* context and also on the tissue type.

As for how the model works, unfortunately the main text gives little information besides that it uses "machine learning." I realize there's a tradeoff between making the main text readable by a general audience and including full details of a method, but it really annoys me that even when computational methods are at the heart of a study, they often get buried in the supplemental material, and this paper is no exception. Digging into the 80+ page supplemental gives some more details: they used an ensemble of neural networks relating RNA features and PSI. The specific features (of which there are more than 1,000) are largely based on their previous work (TODO ref), and include things like short sequence motifs, nucleosome positioning information, "translatability", and more. Readers are referred to previous publications by the authors for more details (TODO refs #11 and #3 from supp).

**Importantly, the model allows prediction of the effects of specific variants on splicing efficiency**: for a given site you can compare predicted PSI for the sequence containing allele A vs. allele B. To show the power of their model for doing so, the authors looked at the predicted effects of common SNPs vs. rare SNPs reportedly associated with disease. They found that intronic disease SNPs are 9x as likely as intronic common SNPs to affect splicing. When restricting to synonymous SNPs, disease SNPs are 9.3x as likely to affect splicing. Interestingly, there was no overall enrichment for disease vs. common missense SNPs, but restricting to positions predicted not to affect protein function showed a 5.6x enrichment for disease vs. common SNPs. GWAS SNPs were not enriched compared to non-GWAS SNPs, but this is likely because the best GWAS SNPs are rarely the actual causal variant in the region.

The authors then go on to look at three disease-specific cases (spinal muscular atrophy, Lynch Syndrome, and autism) . For the first two, they predict effects of sites known to be associated with disease and show good concordance with experimental mutagenesis data. I was a little less convinced by the autism data, but they were able to show that a higher percent of SNPs predicted to affect splicing in cases vs. controls do so in brain, and that these variants are enriched for neurodevelopmental annotations which are not enriched in controls.

## My thoughts

### Predicting functional effects of specific variants: "model-based" approach vs. "annotation overlap" approach
- "computational ‘regulatory models’ that can read the code for any gene and predict relative concentrations of transcripts". much better than just overlapping with annotations. talk about 2 types of ways to assess functionality, reference my GCD blog post
- although i wish the model were more transparent. compare to other choices (e.g. random forests)
"outperform  including linear regression, nearest neighbors and support vector machines (what about random forests?)"

### Does this give us any insight into how splicing works?

### Great effort by authors to provide a web app to make their results useful to others
- whoo uses flask
- nice and shiny
- wish it told me: score for different tissues, ability to export results to text format

### Future directions

* **There's no reason this has to be restricted to SNPs**
* **Fine-mapping association signals**
* **What else can we predict using deep learning from sequence information?**


