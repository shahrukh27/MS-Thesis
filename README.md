# Application of GPD Approach for the *p*-Value Estimation of Modified Peptides

## Overview

Database searching is the most popular approach for peptide identification from tandem mass spectrometry (MS/MS) data. Post-translational modifications alter the structural properties of proteins and add mass shifts in MS/MS spectra, thus hindering the peptide identification. Open database search programs allow search of hundreds of modifications resulting in high false positive rate and slower speed.

To address above concerns in peptide identification, we explored **generalised pareto distribution (GPD) for *p*-value estimation of modified peptides**. 

We followed the following pipeline for estimation of *p*-values using GPD approach:

![alt_text](https://github.com/shahrukh27/MS-Thesis/blob/main/assets/research%20pipeline.png)

## Data Collection

The dataset collection consisted of two components:

* **Input Spectra:** PXD009449 MS/MS dataset was downloaded from the PRoteomics IDEntifications Database (PRIDE).
  
![alt_text](https://github.com/shahrukh27/MS-Thesis/blob/main/assets/MSMS%20Data.png)

* **Target Database:** Human protein sequences were downloaded from UniProtKB database (release # 2022_05; https://www.uniprot.org/proteomes/UP000005640) on Dec 19th, 2022, using Accession ID: UP000005640.
  
![alt_text](https://github.com/shahrukh27/MS-Thesis/blob/main/assets/protein%20database.png)

## Data Cleanup

Peptide and MS/MS Spectra cleaning of PXD009449 dataset is mentioned in detail here: https://github.com/shahrukh27/PXD009449 
We ended up with a total of 78K MS/MS spectra. 

## Database Searching I

MSFragger was used for peptide sequencing and identification and localization of PTMs in 78K input MS/MS spectra. Out of 78K spectra, **70% **had a peptide match in top 5 database hits per spectra, and **58%** had an exact match at the first rank of database hits. Out of the first rank hits, only **29K** had correct PTM identification (excluding spectrum of unmodified peptide) and localization, and an additional **11K** correct hits of unmodified peptides. 

MSFragger results were summarised on these three criteria: peptide identification (PEP), PTM identification (PTM), PTM localisation (PTM LOC). Summarised results are given below:

![alt_text](https://github.com/shahrukh27/MS-Thesis/blob/main/assets/MSFragger%20Results%20Summary.png)

In C0, database hits meet all three criteria i.e., correct peptide identification, correct PTM identification and correct PTM localization (PEP + PTM + PTM LOC). In C1, none of the three criteria are met (- PEP - PTM - PTM LOC) and so on. Only C0 were considered true positives.

## Database Creation

After removing all the duplicates from 5 top database per spectra, we were left with 136,930 peptide. Randomized database was created by applying the following modifications:

* String permutations without replacement
* Random AA substitution at random position
* Deletion at random position
* Random AA insertion at random position

For each MS/MS, among the random database created against each top 5 picks per spectra, 1000 random peptides were selected for phase II of database searching. 

## Database Searching II

Due to tool limitation, we could not run MSFragger on custom random database.

## *p*-value Estimation

Maximum Likelihood (ML) and Probability Weighted Moments (PWM) were used for parameter estimation and Projected Covariance Measure (PCM) and Parameter Estimation Anderson Darling (PAD) for goodness of fit test. We executed 12 GPD executions. We observed in this study as well as other studies in our group that GPD can be used for evaluating identified peptides (modified & unmodified) through database search. Based on our findings, we recommend using PWM for parameter estimation, PCM for goodness of fit.  

## Updates

This research work is continued in my current research work Research Assistant, along with exploration of other database search tools. Results are not mentioned in detail here and will be published later. 

