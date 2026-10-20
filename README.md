# Application of GPD Approach for the *p*-Value Estimation of Modified Peptides

[![Project Masters thesis](https://img.shields.io/badge/Project-Masters_thesis-8062AB)](#overview)
[![Domain Proteomics](https://img.shields.io/badge/Domain-Proteomics-137D87)](#data)
[![Method Generalized Pareto](https://img.shields.io/badge/Method-Generalized_Pareto-3876BB)](#gpd)


## :clipboard: Overview

Database searching is the most popular approach for peptide identification from tandem mass spectrometry (MS/MS) data. Post-translational modifications alter the structural properties of proteins and add mass shifts in MS/MS spectra, thus hindering the peptide identification. Open database search programs allow search of hundreds of modifications resulting in high false positive rate and slower speed.

To address above concerns in peptide identification, we explored **generalised pareto distribution (GPD) for *p*-value estimation of modified peptides**. 

We followed the following pipeline for estimation of *p*-values using GPD approach:

![alt_text](assets/research%20pipeline.png)
<a id="data"></a>
## :floppy_disk: Data Collection

The dataset collection consisted of two components:

* **Input Spectra:** PXD009449 MS/MS dataset was downloaded from the PRoteomics IDEntifications Database (PRIDE).
  [Dataset Details](assets/MSMS%20Data.png)

* **Target Database:** Human protein sequences were downloaded from UniProtKB database (release # 2022_05; https://www.uniprot.org/proteomes/UP000005640) on Dec 19th, 2022, using Accession ID: UP000005640.
  [Target Database Detals](assets/protein%20database.png)

## :broom: Data Cleanup

Peptide and MS/MS Spectra cleaning of PXD009449 dataset is mentioned in detail here: https://github.com/shahrukh27/PXD009449. We ended up with a total of 78K MS/MS spectra. 

## :mag: Database Searching I

MSFragger was used for peptide sequencing and identification and localization of PTMs in 78K input MS/MS spectra.  

| Outcome | Result stated in the original summary |
| :---: | :---: |
| Peptide match among the top five hits | Approximately 70% of the processed spectra |
| Exact peptide match at rank one | Approximately 58% of the processed spectra |
| Rank-one modified-peptide hits with correct PTM identity and localization | Approximately 29,000 |
| Additional correct rank-one unmodified-peptide hits | Approximately 11,000 |

MSFragger results were summarised on these three criteria: peptide identification (PEP), PTM identification (PTM), PTM localisation (PTM LOC). Only **C0** were considered true positives.

| Category | Peptide identity | PTM identity | PTM location |
| :---: | :---: | :---: | :---: |
| C0 | Correct | Correct | Correct |
| C1 | Incorrect | Incorrect | Incorrect |
| C2 | Incorrect | Incorrect | Correct |
| C3 | Incorrect | Correct | Correct |
| C4 | Correct | Incorrect | Incorrect |
| C5 | Correct | Correct | Incorrect |
| C6 | Incorrect | Correct | Incorrect |
| C7 | Correct | Incorrect | Correct |

[Summarized Results](assets/MSFragger%20Results%20Summary.png)

## :file_cabinet: Database Creation

After removing all the duplicates from 5 top database per spectra, we were left with 136,930 peptide. Randomized database was created by applying the following modifications:

* String permutations without replacement
* Random AA substitution at random position
* Deletion at random position
* Random AA insertion at random position

For each MS/MS, among the random database created against each top 5 picks per spectra, 1000 random peptides were selected for phase II of database searching. 

## :mag: Database Searching II

Due to tool limitation, we could not run MSFragger on custom random database.
<a id="gpd"></a>
## *p*-value Estimation

| Role | Methods reported on the original page |
| :---: | :---: |
| Parameter estimation | Maximum Likelihood (ML); Probability Weighted Moments (PWM) |
| Goodness-of-fit assessment | Projected Covariance Measure (PCM); Parameter Estimation Anderson Darling (PAD) |
| Number of GPD runs | 12 |
| Recommendation reported by the project | PWM for parameter estimation and PCM for goodness-of-fit assessment |

## :placard: Updates

This research work is continued in my current research work Research Assistant, along with exploration of other database search tools. Results are not mentioned in detail here and will be published later. 

