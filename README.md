# MGBB-GSA-QC

This repository details the quality control (QC) pipeline for the imputation data of MGB Biobank samples, which are genotyped on Illumina’s GSA array. The pipeline and parameter recommendations described here follow the guidelines provided in [this repository](https://github.com/getian107/MGBB-QC/), which details the QC pipeline for Illumina’s GSA genotyped data of MGB Biobank samples.

The dataset includes the imputation data of 53,253 individuals in hg38 coordinates. The samples are initially genotyped on Illumina's GSA array in hg19 coordinates across 4 different batches (batches A to D). After imputation using TOPMed on Michigan server, data were converted from VCF dosages to PLINK hard-call genotypes. Post-imputation data from 4 batches were merged.

## Quality control pipeline

- Post-imputation QC (with *PLINK*)

  - INFO score/Imputation R2 >0.6
  - MAF (based on imputed dosages) >0.5%
  - HWE >1e-10
  - SNP-level call rate >0.95
  - Remove SNPs that show batch associations
    - Regress each batch indicator (coded as 1 to 4) on SNPs, adjusting for sex (association P < 1e-4)

## Summary of post-imputation QC

Samples are genotyped and imputed on 4 batches:
- Batch 0110: 13,126 samples
- Batch 0111: 11,642 samples
- Batch 0112: 5,972 samples
- Batch 0113: 22,514 samples

### Variant Filter Results
  
  | Variant filter       | # Variants       | % Total       |
  | --------- | --------- | --------- |
  | INFO score/Imputation R2 >0.6     | 163,935,146    | 100%     |
  | Call rate < 0.95     | 116,310,366     | 70.9%     |
  | HWE < 1e-10     | 3,329,462     | 2.0%     |
  | MAF < 0.5%     | 35,269,823     | 21.5%     |
  | Showing batch association (p < 1e-04)     | 85,570     | 0.05%     |
  | ***Post-QC***        | 8,939,925 | 5.55%    |

## Ancestry assignment

We followed the the pipeline recommended by **POP-MaD** to perform ancestry assignment. 

We only used the genotyped variants for the ancestry assignment. 
- Since genotyped variants are in hg19 coordinates, we first used *LiftOver* to convert the coordinates from hg19 to hg38.
- 4 batches' genotyped variant lists were merged (overlapping genotyped variants between 4 batches)
- We extracted genotyped variants from post-QC imputation data by *PLINK*

The dataset includes 444067 variant info for 53253 individuals is used in the following ancestry assignment processes.
