---
layout: post
title: Notes on Xiong, et al. "The human splicing code reveals new insights into the genetic determinants of disease"
published: true
tags: science
categories: science
date: 2014-12-31 00:00:00
---

**Summary**: My thoughts on the recent paper by [Xiong et al.](http://www.sciencemag.org/content/early/2014/12/17/science.1254806.full) published in *Science* this month. Overall I think this study is very promising in showing how we can build powerful model-based predictions of gene regulation using sequence features. 

## Summary of the study

Xiong et al. build a model that uses nearby sequence features to predict for a triplet of exons, the percent of transcripts for which the central exon is spliced in (PSI) for a specific tissue. Although building such a model has been attempted before by the same authors (e.g. [Barash et al 2010](http://www.nature.com/nature/journal/v465/n7294/full/nature09000.html), and [Xiong et al 2011](http://bioinformatics.oxfordjournals.org/content/27/18/2554.long)), the method published here seems to perform significantly better, and includes a more comprehensive set of features and outputs absolute PSI values per tissue. Their model captures effects such as those of known RNA binding proteins that act in *cis* as well as *trans* acting factors, and shows that the effect of different sequence features depends both on the *cis* context and also on the tissue type.

As for how the model works, unfortunately (as is usually the case in such studies...) the main text gives little information besides that it uses "machine learning." Digging into the 80+ page supplemental material gives some more details: they used an ensemble of neural networks relating RNA features and PSI. The specific features (of which there are more than 1,000) are largely based on [their previous work](http://www.nature.com/nature/journal/v465/n7294/full/nature09000.html), and include things like short sequence motifs, nucleosome positioning information, "translatability", and more. They refer readers to their previous publications for more details of the model ([Barash et al 2010](http://www.nature.com/nature/journal/v465/n7294/full/nature09000.html), and [Xiong et al 2011](http://bioinformatics.oxfordjournals.org/content/27/18/2554.long)).

**Importantly, the model allows prediction of the effects of specific variants on splicing efficiency**: for a given site you can compare predicted PSI for the sequence containing allele A vs. allele B, which is pretty powerful. To evaluate their model, the authors looked at the predicted effects of common SNPs vs. rare SNPs reportedly associated with disease. They found that intronic disease SNPs are 9x as likely as intronic common SNPs to affect splicing. When restricting to synonymous SNPs, disease SNPs are 9.3x as likely to affect splicing. Interestingly, there was no overall enrichment for disease vs. common missense SNPs, but restricting to missense SNPs predicted not to affect protein function showed a 5.6x enrichment. GWAS SNPs were not enriched compared to non-GWAS SNPs, but this is likely because the best GWAS SNPs are rarely the actual causal variant in the region.

The authors then showed the power of their model to predict effects of specific variants by looking at three disease-specific cases (spinal muscular atrophy, Lynch Syndrome, and autism). For the first two, they predict effects of sites known to be associated with disease and showed pretty impressive concordance with experimental mutagenesis data. For autism, they showed that a higher percent of SNPs predicted to affect splicing in cases vs. controls do so in brain, and that these variants are enriched for neurodevelopmental annotations.

## My thoughts

### Model-based approaches for predicting functional effects of specific variants are very powerful

Overall I was very excited about the ability of this model to predict the functional impact of specific variants on splicing. One of the big challenges in the post-GWAS era is fine mapping association signals to determine true causal variants. People are tackling this problem in a number of ways. One simple approach is to simply overlap variants with known functional annotations, such as DNAse HS, transcription factor binding sites, chromatin information, eQTLs, and more. A more useful approach is to build probabilistic models that incorporate these annotations to determine the posterior probability of a site to be the causal variant. Great work on this has been done by [Bogdan Pasaniuc's group](http://www.plosgenetics.org/article/info%3Adoi%2F10.1371%2Fjournal.pgen.1004722), [Joe Pickrell](http://www.cell.com/ajhg/abstract/S0002-9297%2814%2900106-2), and others.

Another type of approach is to use "deep learning" techniques from machine learning to directly predict regulatory characteristics of variants based on a large number of sequence features, which the authors do here. While I wish the model were more transparent (see below), the ability to take a specific position in the genome and predict the functional outcome of changing the nucleotide that position is extremely powerful. With such a tool, we could look at the plausible set of causal variants for a GWAS locus, predict the effect of each one, and hopefully get a good idea of the true causal site. Splicing is of course not the whole story, but this study is an example of such an approach, and as the authors mention in the discussion a similar approach can likely be used to predict other important regulatory features.

### Does this give us any insight into how splicing works?

I'm convinced that this model gives quite good predictions of splicing efficiency across a variety of contexts. Although the exact modeling details were not very clearly described, the point is not about this specific model. I'm guessing other machine learning approaches, such as random forests, might achieve similar performance. The point is that because this worked, we know that splicing, and probably many other regulatory features, are at least partially "predictable" using information we know from sequence data, which is pretty exciting and promising for variant interpretation.

But what I did not get from reading this paper was that we learned any new insights into how splicing works. It would have been great if they discussed what the most informative features turned out to be. Did unexpected or novel features play a big role? Is there a clear set of "important" features that teaches us about the splicing code, or is the answer it's complicated and only by combining hundreds of features into a black box (neural network) can we get good predictions?

### Great effort by authors to provide a web app to make their results useful to others

The authors set up a [shiny web application](http://tools.genes.toronto.edu/) where users can upload their own set of SNPs and get predictions about their effects on splicing. This makes their results infinitely more useful and accessible and I wish more papers did this, so big kudos! Otherwise the method would be buried in the supplemental forever and nobody would ever use it.

I found their website very easy to use. I do wish that it gave scores by tissue and that it had the ability to export results to a text format for further processing.

### Future directions

I think there is a lot of room to apply similar predictive methods to a wide range of important problems:

* **There's no reason this has to be restricted to SNPs**. The authors only talk about single nucleotide changes in the paper, but one could easily apply the same method to predict regulatory changes caused by other variant types, such as indels or short tandem repeats. These have been much less explored and there is potentially a lot to learn from doing this.
* **Fine-mapping association signals**. As I discussed above, we can start to use this type of approach to help in fine-mapping: i.e. predict the functional effect of all variants tagged by a GWAS SNP to determine likely candidates for causal variants.
* **What else can we predict using deep learning from sequence information?**. Lots of things. The authors point out a few in the discussion (chromatin, transcription, polyadenylation, mRNA turnover, translation, protein stability, etc. likely all can be predicted to some extent using sequence features). The features will be different but the methods will be similar. If we can build good models of these processes we can start to build a great toolkit for predicting effects of regulatory variation in the genome, and hopefully gain biological insights along the way.

In summary I'm optimistic about the powerful predictions made by this type of approach and think it holds good promise for interpreting results in the post-association-study era.