http://www.sciencemag.org/content/early/2014/12/17/science.1254806.full

points:

* I like the idea of predictive models to determine functionality of different sites
- "computational ‘regulatory models’ that can read the code for any gene and predict relative concentrations of transcripts". much better than just overlapping with annotations. talk about 2 types of ways to assess functionality, reference my GCD blog post
- main text just says they used "machine learning"
* does this tell us about insights into splicing that we didn't have before?
* annoys me that main text of papers like this doesn't give any clue how the method works
* does it work for indels? other variant types?
* how compare to previous models (including their own?)
* try using their tool and comment on this
- whoo uses flask
- nice and shiny
- wish it told me: score for different tissues, ability to export results to text format


summary:
## A computational model of splicing
model can be applied to any triplet of exons: extract sequence features, for given cell type predict PSI
find captures effects of known RNA binding proteins well: RNABP affinity (from Ray et al.) not correlated with residual errors. (but are those used as features? if yes isn't this expected?)
also look at trans-factors (Muscle-blind like RNA BP)
find predictions are context-specific (tissue). many features switch direction of effect depending on context
look at LCLS (Geuvadis) and find can predict differences in psi based on genotype (correctly predict direction. what about magnitude of effect?)

## Genome-wide analysis of splicing misregulation and disease
look at a bunch of common, also rare disease SNPs in reg. regions. apply model to sequence with and without SNP, compute change in PSI for each tissue. look at largest delta, also aggregate across tissues
find ~20,000 SNPs affecting splicing, often depends on cis-context. intronic snps matter, so do missense/nonsense as well as synonymous
intronic disease SNPs more than 30bp from splice site 9x as likely as common SNPs to affect splicing. 9.3x for synonymous SNPs. no diff for missense (makes sense, missense more likely affecting protein coding. ) but if restrict to those minimally affecting protein function, get 5.6x. GWAS SNPs not enriched compared to non-GWAS (likely because they're not the causal variant? can you use this to fine-map?)

## Disease-specific
### Spinal muscular atrophy (SMA)
predict effects of 4 sites differ between SMN1/2. backup with minigene reporter
compare to mutagenesis data, find correct direction 85% of time

### Nonpolyposis colorectal cancer
Lynch Syndrome mismatch repair genes
again compare to experimental data, find concordance between model predicting increased skipping vs. no change, 93% AUC. unclear how they chose which SNPs are "disease" here and elsewhere? looks like previously associated in literature

### Autism spectrum disorder (ASD)
look at 5 idiopathic ASD cases. get brain samples from these and from 12 controls. 42K "rare" SNPs per subject. apply model
find higher percent of SNPs causing misregulated to affect brain in autism vs. controls (not super convincing to me)
top misregulated genes enriched for neurodev categories in ASD samples, not in controls

## Discussion
much better than just overlapping with annotations
unlike GWAS, QTL, does not depend on allele frequencies
can use to fine-map GWAS/QTL signals
"We anticipate that it will be important to seek regulatory models that encompass other major steps in gene regulation, including chromatin dynamics, transcription, polyadenylation, mRNA turnover, protein synthesis, and protein stabilization". apply similar approach to predicting other processes. can even model these jointly

## Methods - how does it work?
ensemble of neural network models relating RNA features and PSI
each model tries to maximize "code quality": info provided by model above naive guesser
single model in the ensemble is a two-layer neural network with sigmoidal hidden units
tissues are separate output units
use MCMC
outperform  including linear regression, nearest neighbors and support vector machines (what about random forests?)

## Other
look into their previous paper and compare
calculated RBP binding affinity from Ray et al. (14): look at this to see if patterns for STRs
how much seq included? the two central introns?

# Previous paper
http://www.nature.com/nature/journal/v465/n7294/full/nature09000.html 2010
look at exon triplet, plus internal introns up to 300nt away from splice site
1,014 features of four types: known motifs, new motifs, short motifs and features describing transcript structure
fig 3a shows features with strongest predictive power. missing this from current paper