---
layout: post
title: Notes on Gamazon, Wheeler, and Shah, et al. "PrediXcan&#58; Trait Mapping Using Human Transcriptome Regulation"
published: true
tags: science
categories: science
date: 2015-06-30 00:00:00
---

**Summary**: My notes on a paper recently posted on *biorXiv* by [Gamazon, Wheeler, and Shah et al.](http://biorxiv.org/content/early/2015/06/17/020164) about using imputed expression values to map human traits. Overall I think this is a clever and powerful method for providing biological insight into GWAS signals. However I think there is still a lot of work to be done in effectively incorporating transcriptomic data into GWAS.

## Summary of the study

Gamazon, Wheeler, and Shah et al. present *PrediXcan*, a method to directly test for association of a gene with a trait by using imputed expression values as input to association tests. (side note, GWaS would be a great acronym for the list of first authors, but that would get way too confusing for the following discussion). PrediXcan first uses a reference transcriptome dataset to learn a model of the effect of each *cis* SNP on expression of nearby genes. It then imputes gene expression values into an independent dataset, and uses the imputed values to perform association tests. The resulting signals avoid the problem of determining which gene a GWAS hit is relevant to by directly implicating a specific gene.

As pointed out by the authors, this method has clear advantages over simple GWAS:

* **Directly testing functional units** In a vanilla GWAS, the resulting hits provide little biological insight. One simple hypothesis is that a SNP most likely influences the gene in closest proximity. However, this assumption can clearly be violated, as is the case for the well-known *FTO* locus for obesity, for which it was recently shown the signal is actually [driven by long range interactions with a more distant gene, *IRX3*](http://www.nature.com/nature/journal/v507/n7492/full/nature13138.html). By aggregating SNPs into units that are known *a prior* to be functional, this method directly implicates specific genes.

* **The multiple hypothesis burden is greatly reduced** by testing genes rather than each SNP individually. (However it is unclear how multiple testing is handled when testing many tissues at a time). This potentially increases power to detect associations.

* **Tissue relevance**. PrediXcan can learn models for different tissues, and therefore can learn which tissues the relevant genes are affected in. I would have liked to see more about this (see below).

* **Direction of effect**. By using expression values as input to assiciation tests, we have the opportunity to learn not only which genes drive a signal, but also the *direction* of effect: is up or down-regulation of this gene associated with higher risk for a certain trait?

* **No transcriptome data from your samples is required**. By relying on a reference transcriptome and using imputed expression levels, PrediXcan can be used for samples where expression data is not actually available.

The authors first develop their method for expression prediction. They tested several methods but decided to use an elastic to learn weights for each SNP. They trained this on whole blood from the Depression Genes and Network (DGN) dataset and used 10-fold cross to evaluate their performance. The average R<sup>2</sup> between the predicted and observed values was 13.7%, quite close to the upper bound of 15.3% average heritability across these genes. However, the performance of their model on independent expression datasets (from GTEx and Geuvadis) on the other hand was not so impressive (R<sup>2</sup> values closer 1o 1-5%). I discuss more concerns about this below.

They used their expression model on whole blood to impute expression in samples from WTCCC and used logistic regression to perform association tests for 7 complex traits. They found 41 significant associations across 5 traits. As expected, most of these associations are near previously reported signals. Some novel associations are reported (e.g. DCLRE1B in T1D and rheumatoid arthritis), and other previous associations are recovered (e.g. PTPN22 in T1D and RA) with the added benefit of getting information about the direction of effect.

## My Thoughts - TODO

0. Overall: clever and Powerful method with clear advantages
- obvious thing would be to use expr x trait. but run into issue of causality. but here since focus on grex, this shouldn't be an issue
- will only work on genes with eqtls. how many gwas hits are nowhere near an eqtl gene?
- wish there were more examples where the gene not obvious. FTO is key example: should show here what you do on that!
- will this explain additional h2? no. don't expect this to lead to many new hits or explain any more h2

concerns/things I'd like to see:
1. reproducibility of expression estimates
- how much does training set matter? If I train on gtex instead will I get a different result?
- how concordant are the weights between studies? what about h2 estimates?
- what about the R2 for model vs. different datasets? show 10-fold cross validation is pretty good (0.137 compared to upper limit of h2=0.153)
- used weights from DGN whole blood to predict geuvadis LCLs. why? we know they're different. QQ plot of r2 seems a pretty ridiculous way to show this, of course we sure hope the r2's are different than the null distribution. why not show histogram of the r2s?
- average r2 0.0197 for geuvadis lcls, a lot lower than the 0.137 in same dataset. For GTEx, higher in all, highest in whole blood. why are r2 so much lower in different datasets? different populations? technical variation? different normalization applied to each? this is a huge difference
data
- dgn: downloaded normalized gene level data
- geuvadis: raw data? norm?
- gtex: adjust for gender 3 pcs 15 peer factors
- what about different populations?

2. Do more with tissue specificity
expected more on this, given how big of an advantage it is
useful in learning about relevant tissue: also wish they showed: can use this to figure out tissue specificity by looking for enrichment of signal in various tissues
- why did they use DGN whole blood to test in each GWAS, why not get in GTEx all tissues then look at all of those? what about comparing when different reference transcriptomes are used? 

3. Other minor points
- how many SNPs per gene are useful? 1? more? how many features does the LASSO choose on average? are the same SNPs used in different datasets?
- figure 7 not convincingly better than SKAT (look up how this works). just a few hits are higher
- what's going on with the components of gene expr?
break up gene expression into components:
1. GReX
2. altered by trait itself (reverse causal) what is the motivtation for this? how much weight does this usually get? potentially very interesting, but not discussed at all
3. environmental/other factors

## Future directions - TODO
future directions: apply to all gwas's. get relevant genes, tissues of interest											is there a way to do this without the intermediate gene expression value? like take all assoc with expression, use variance partitioning? will lose information with this intermediate calculation, inherently noisy. (is this what they refer to as attenuation bias?)