# MGBB-GSA-QC

This repository details the quality control (QC) pipeline for the TOPMed imputated data of the MGB Biobank samples. The dataset includes the imputated data of 53,253 individuals in hg38 coordinates. The samples are initially genotyped on Illumina's GSA array in hg19 coordinates across 4 different batches (batches A to D). After imputation using the TOPMed reference panel on the TOPMed imputation server, data were converted from VCF dosages to PLINK hard-call genotypes. Post-imputation data from 4 batches were merged. The post-imputation QC parameters described here follow the guidelines provided in [this repository](https://github.com/getian107/MGBB-QC/).

## Quality control pipeline

- Post-imputation QC (with *PLINK*)

  - INFO score/Imputation R2 >0.6
  - MAF (based on imputed dosages) >0.5%
  - HWE >1e-10
  - SNP-level call rate >0.95
  - Remove SNPs that show batch associations
    - Four independent GWAS were performed, with each analysis employing a binary coding scheme to compare one chip versus the remaining three chips, adjusting for sex (association P < 1e-4)
    - Variants showing potential association with batch effect, as identified in each of the four GWAS, were subsequently pooled and removed from downstream analysis

## Summary of post-imputation QC

Samples are genotyped and imputed on 4 batches:
- Batch 0110: 13,126 samples
- Batch 0111: 11,642 samples
- Batch 0112: 5,972 samples
- Batch 0113: 22,513 samples

### Variant Filter Results
  
  | Variant filter       | # Variants       | % Total       |
  | --------- | --------- | --------- |
  | INFO score/Imputation R2 >0.6     | 163,935,146    | 100%     |
  | Call rate < 0.95     | 116,310,366     | 70.9%     |
  | HWE < 1e-10     | 3,329,462     | 2.0%     |
  | MAF < 0.5%     | 35,269,823     | 21.5%     |
  | Showing batch association (p < 1e-04)     | 283,068     | 0.17%     |
  | ***Post-QC***        | 8,742,427 | 5.43%    |

## Ancestry assignment

We followed the pipeline recommended by **POP-MaD** to perform ancestry assignment. 

We only used the genotyped variants for the ancestry assignment. 
- **Coordinate Conversion**: Since genotyped variants are in hg19 coordinates, we first used *LiftOver* to convert the coordinates from hg19 to hg38.
- **Data Merging**: Genotyped variant lists from 4 batches were merged (focusing on overlapping genotyped variants between 4 batches).
- **Variant Extraction**: We extracted genotyped variants from the post-imputation data (the dataset qc'ed only using INFO score R2 > 0.6) by *PLINK*.
- **Remove Batch effect**
  - Four independent GWAS were performed, with each analysis employing a binary coding scheme to compare one chip versus the remaining three chips, adjusting for sex (association P < 1e-4)
  - Variants showing potential association with batch effect, as identified in each of the four GWAS, were subsequently pooled and removed from downstream ancestry assignment analysis

### Variant Filter Results
  
  | Variant filter       | # Variants       | % Total       |
  | --------- | --------- | --------- |
  | Genotyped variants     | 637,836    | 100%     |
  | Showing batch association (p < 1e-04)     | 54,714     | 8.58%     |
  | ***Post-QC***        | 583,122 | 91.42%    |

The dataset includes 583,122 variant info for 53,253 individuals and is used in the following ancestry assignment processes.

### Ancestry Assignment Results

We calculated the Mahalanobis distance of each individual (w/ 10 PCs) from outlier-pruned ref panel pop (KGP_HGDP) and filtered outliers by SD.

| SD       | AFR       | AMR       | CSA | EAS | EUR | MID | OCE | Unassigned |
  | --------- | --------- | --------- | --------- | --------- |--------- |--------- |--------- |--------- |
  | raw     | 2314    | 4115     | 774  | 1012 | 44280 | 756 | 2 | - |
  |  4     | 2293    | 4078     | 770 | 1004 | 44056 | 747 | 2 | 303 |
  |  3     | 2253     | 4043     | 761 | 1001 | 43980 | 746 | 2 | 467 | 
  |  2        | 2143 | 3935    | 669 | 985 | 43769 | 744 | 2 | 1006 |
  |  1        | 1941  | 3419    | 535 | 961 | 40506 | 672 | 2 | 5217 |
