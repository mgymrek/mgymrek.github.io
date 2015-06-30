---
layout: post
title: Notes on Gamazon, Wheeler, and Shah, et al. "PrediXcan&#58; Trait Mapping Using Human Transcriptome Regulation"
published: true
tags: science
categories: science
date: 2015-06-29 00:00:00
---

**Summary**: My notes on a paper recently posted on *bioRxiv* by [Gamazon, Wheeler, and Shah et al.](http://biorxiv.org/content/early/2015/06/17/020164) about using imputed expression values to map human traits. Overall I think this is a clever and powerful method for providing biological insight into GWAS signals. However I think there is still a lot of work to be done in effectively incorporating transcriptomic data into GWAS.

## Summary of the study

Gamazon, Wheeler, and Shah et al. present *PrediXcan*, a method to directly test for association of a gene with a trait by using imputed expression values as input to association tests. (side note, GWaS would be a great acronym for the list of first authors, but that would get way too confusing for the following discussion). PrediXcan first uses a reference transcriptome dataset to learn a model of the effect of each *cis* SNP on expression of nearby genes. It then imputes gene expression values into an independent dataset, and uses the imputed values to perform association tests. The resulting signals avoid the problem of determining which gene a GWAS hit is relevant to by directly implicating a specific gene.

As pointed out by the authors, this method has clear advantages over simple GWAS:

* **Directly testing functional units.** In a vanilla GWAS, the resulting hits provide little biological insight. One simple hypothesis is that a SNP most likely influences the gene in closest proximity. However, this assumption can clearly be violated, as is the case for the well-known *FTO* locus for obesity, which was recently shown to be [driven by long range interactions with a more distant gene, *IRX3*](http://www.nature.com/nature/journal/v507/n7492/full/nature13138.html). By aggregating SNPs into units that are known *a priori* to be functional, this method directly implicates specific genes.

* **The multiple hypothesis burden is greatly reduced** by testing genes rather than each SNP individually. (However it is unclear how multiple testing is handled when testing many tissues at a time). This can increase power to detect associations.

* **Tissue relevance**. PrediXcan can learn models for different tissues, and therefore can learn which tissues the relevant genes are affected in.

* **Direction of effect**. By using expression values as input to assiciation tests, we have the opportunity to learn not only which genes drive a signal, but also the *direction* of effect: is up or down-regulation of this gene associated with higher risk for a certain trait?

* **No transcriptome data from your samples is required**. By relying on a reference transcriptome and using imputed expression levels, PrediXcan can be used for samples where expression data is not actually available.

The authors first develop their method for expression prediction. They tested several methods but decided to use an elastic net to learn weights for each SNP. They trained this on whole blood from the Depression Genes and Networks (DGN) dataset and used 10-fold cross validation to evaluate their performance. The average R<sup>2</sup> between the predicted and observed values was 13.7%, quite close to the upper bound of 15.3% average heritability measured across these genes. However, the performance of their model on independent expression datasets (from GTEx and Geuvadis) on the other hand was not so impressive (R<sup>2</sup> values closer 1o 1-5%). I discuss more about this below.

They used their expression model on whole blood to impute expression in samples from WTCCC and used logistic regression to perform association tests for 7 complex traits. They found 41 significant associations across 5 traits. As expected, most of these associations are near previously reported signals. Some novel associations are reported (e.g. DCLRE1B in T1D and RA), and other previous associations are recovered (e.g. PTPN22 in T1D and RA) with the added benefit of getting information about the direction of effect.

## My Thoughts

Expression data can give us a more direct idea than regular GWAS of which genes are important. One obvious thing to try would be to associate measured gene expression values with a trait. However, this approach breaks a key point of GWAS, namely the direction of causality is not clear. When correlating a SNP with a trait, we can be reasonably sure which direction causality goes, since the trait is unlikely to affect the underlying genotypes. However, it's not clear this works with expression. It could very well be for instance that having diabetes changes expression of some genes. Then naively associated expression with diabetes would reveal these genes as hits, even if they are not underlying causes of the disease. **PrediXcan cleverly gets around the problem of reverse causality by focusing on the component of gene expression that is genetically regulated.**

Will this approach explain addition heritability? The answer must be no. We can't create information from nothing, and converting SNP genotypes to gene expression values will not be able to explain any more heritability than just using the SNPs in the first place (in fact surely we will lose some information in this conversion). Will this approach find new signals? Maybe, perhaps due to the increase in power with a lower multiple hypothesis burden. But for the most part this will give biological insight into existing samples: which genes are important? what is the direction of effect? Which tissues are important? 

Overall, I think PrediXcan is a clever and powerful method that gives additional information on top of SNP-based GWAS scans. Despite its advantages though, I think there is still a lot of work to be done to effectively incorporate transcriptome data (and other omics data) into GWAS interpretation. I discuss some of my specific concerns and things I would have liked to see more of below.

### 1. More specific examples
I wish the authors had shown some clear examples where the implicated gene was not obvious from the original GWAS scan. The *FTO* case is a key example: was PrediXcan able to pull out *IRX3*? Was it able to determine the relevant tissue type? I would have like to see this and perhaps other cases where PrediXcan clearly gave us the right answer that wasn't obvious beforehand.

### 2. Expression values depend heavily on the training dataset
The authors primarily use SNP weights trained on the DGN whole blood samples. How much does the training dataset matter? If I now train this on GTEx or Geuvadis will I get similar weights and expression predictions?

It seems like the answer is that the training set matters quite a bit. While imputation seems to work quite well when performing cross-validation on DGN samples (average R<sup>2</sup>=0.137 compared to the upper limit defined by an average heritability of 0.153), the picture looks different when using a model trained on DGN to impute expression in other datasets.  

The DGN whole blood model was used to predict expression for Geuvadis and GTEx LCL samples (note, whole blood and LCLs are not quite the same thing, and they are expected to have different eQTLs and therefore different SNP weights, so we know there should be some differences). The average R<sup>2</sup> in the Geuvadis data was 0.0197, compared to 0.137 in the cross validation analysis. GTEx was a little better, with values between 1-5% across 9 tissues, with whole blood reassuringly showing the best performace. So expression imputation is performing significantly worse when using a model trained on one dataset to impute expression in another.

The results of the validation exercises were displayed in a peculiar way: Figure 4 shows a QQ plot comparing the expected distribution of R<sup>2</sup> under the null vs. the observed distribution of R<sup>2</sup>'s across genes. The observed R<sup>2</sup>'s show strong departure from the null expectation, which I sure hope is the case! Otherwise if I'm interpreting this right it would mean that one eQTL dataset gives essentially no information about eQTLs in another dataset and all hope is lost. What would have been more informative would be a simple histogram of R<sup>2</sup> values across all genes.

Although discouraging, the poor reproducibility of models across expression datasets is perhaps not surprising. It is well known that transcriptome data exhibits a large amount of noise due to technical variation, and people have spent entire careers on how to deal with this. One important consequence is that the normalization and processing details matter. Based on the methods, the expression datasets were all processed using distinct pipelines (and contained individuals with different population histories), which surely introduces variability (for DGN, they downloaded normalized gene level data, for Geuvadis they presumably downloaded normalized expression values but this wasn't clear, and for GTEx they performed their own processing by adjusting for gender, 3 top genotype principal components, and 15 PEER factors). I would like to see how well this performs between independent datasets that have been uniformly processed. Or between microarry vs. RNAseq data performed on the same individuals. However, even if that does greatly increase the concordance, it is concerning how much normalization influences the imputed expression values, and therefore downstream association tests. I think a lot of work needs to be done to figure out the best way to do this.

### 3. The ability to determine tissue specificity is not explored

A key advantage of this method should be that it can provide tissue specific information. If a gene is associated with a trait in some tissues but not others, that is a good indication that a specific tissue plays a key role. Additionally, one could look at the distribution of p-values for associations across tissue types to determine the relevant tissue type. 

The authors focused primarily on expression values imputed from the DGN whole blood data, and did not explore whether PrediXcan indeed gives informative information about relevant tissues. Why not use GTEx as the reference transcriptome and use this to do associations with expression from different tissues?

### 4. Other minor points

* On average how many SNPs per gene were useful for prediction (e.g. had non-zero weights in LASSO or elastic net)? Does the best SNP capture most of variation explained by the model? (If so, there is not much point in converting to expression values, one should just use that SNP). Do these results fit with what we know about eQTLs?

* The authors claim that they break up expression into three components: (1) GReX (genetically regulated expression) (2) A reverse causality component representing an affect of the trait itself on expression and (3) environment/other factors. However as far as I can tell they never talk about these components again. In particular, component (2) sounds very interesting. Is this actually measured in this study? If so how much weight does it get?

## Future directions

An obvious future direction is to impute expression across many tissues in available GWAS datasets to determine which genes are driving signals, their directions of effect, and relevant tissue types.

Another direction is to figure out how to combine information learned from expression data with fine-mapping approaches based on functional annotations of individual SNPs (e.g. work done by [Bogdan Pasaniuc's group](http://www.plosgenetics.org/article/info%3Adoi%2F10.1371%2Fjournal.pgen.1004722), [Joe Pickrell](http://www.cell.com/ajhg/abstract/S0002-9297%2814%2900106-2), and others).

One other point is whether there is a way to do this without actually needing to calculate an intermediate expression value? The imputed values themselves seem to be poorly reproducible across studies, and information will be lost after the noisy conversion of SNP genotypes to expression. I think it's worth figuring out how to get around doing this.

In summary this is a good step toward incorporating transcriptomic data into GWAS, and it seems that integrating approaches like this with other fine-mapping techniques will reveal new insights into unexplained GWAS loci and relevant tissue types driving different traits. 