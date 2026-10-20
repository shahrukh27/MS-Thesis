# MS Thesis
### GPD-based p-value estimation for modified peptide identifications

[![Project Masters thesis](https://img.shields.io/badge/Project-Masters_thesis-8062AB)](#overview)
[![Domain Proteomics](https://img.shields.io/badge/Domain-Proteomics-137D87)](#data)
[![Method Generalized Pareto](https://img.shields.io/badge/Method-Generalized_Pareto-3876BB)](#gpd)

<a id="overview"></a>

This repository summarizes my Master's thesis work on using the **generalized Pareto distribution (GPD)** to assess the statistical significance of peptide identifications, including post-translational modifications (PTMs), from tandem mass spectrometry (MS/MS) data.

The research combines database-search results, randomized peptide candidates, and GPD-based statistical assessment. The page documents the first MSFragger search and candidate-generation procedure, reports 12 GPD runs, and records a limitation that prevented the second MSFragger search against the custom randomized database.

![Research workflow separating the documented first search and candidate generation from the blocked second search and separately reported GPD assessment](assets/ms-thesis/research_overview.png)

**Research question:** can a GPD-based approach help assess the significance of modified peptide identifications?  
**Data:** processed PXD009449 spectra and a human UniProtKB search database.  
**Repository scope:** a research summary and explanatory figures. The original analysis scripts and per-run GPD outputs are not currently included.

[Explore the method](#gpd) · [Understand the data](#data) · [View reported results](#results) · [Thesis citation](#thesis-citation)

## 🎬 Understanding the GPD approach

![Teaching animation showing randomized match scores, a tail threshold, an illustrative GPD tail model, and an example observed score](assets/ms-thesis/gpd_tail_walkthrough.gif)

*Conceptual illustration using synthetic scores and known illustrative parameters. It is not a fit to thesis data or a reconstruction of the 12 reported GPD runs. Higher scores are assumed to be more extreme in this example.*

[MP4 version](assets/ms-thesis/gpd_tail_walkthrough.mp4) · [Static version](assets/ms-thesis/gpd_tail_walkthrough_poster.png)

## 🔬 Why modified peptides are challenging

Database searching compares an observed MS/MS spectrum with candidate peptide explanations. A post-translational modification can alter a peptide's mass and fragmentation pattern, making it important to distinguish three questions:

1. Is the peptide sequence correct?
2. Is the modification correctly identified?
3. Is the modification assigned to the correct residue?

The thesis explored GPD-based p-value estimation as a statistical assessment approach for these identifications. A p-value concerns the extremeness of a score under a specified null model; it is not the posterior probability that an identification is incorrect, and it does not by itself validate PTM localization.

<a id="data"></a>

## 🗂️ Data and search database

![Input spectra from PXD009449 and the human UniProtKB target and decoy search database](assets/ms-thesis/data_sources.png)

| Component | Details reported in the project |
| --- | --- |
| Spectral dataset | [PXD009449 in PRIDE](https://www.ebi.ac.uk/pride/archive/projects/PXD009449) |
| Dataset content | Approximately 5,000 synthetic peptides, 21 PTMs, and corresponding unmodified forms |
| Fragmentation | Collision-induced dissociation (CID) and higher-energy collisional dissociation (HCD) |
| Processed spectra | Approximately 78,000 MS/MS spectra |
| Protein database | Human UniProtKB proteome [UP000005640](https://www.uniprot.org/proteomes/UP000005640) |
| Historical database version | Release 2022_05, downloaded on 19 December 2022 |
| Decoy construction | Reversed protein sequences, as described in the original workflow |

Peptide and spectrum filtering is documented in the companion [PXD009449 repository](https://github.com/shahrukh27/PXD009449). The historical dataset and protein-database illustrations remain available as [spectral dataset details](assets/MSMS%20Data.png) and [protein database details](assets/protein%20database.png).

The current UniProt page is a source link; reproducing this study requires the historical release and retrieval settings, not a new download treated as an identical database.

## ⚙️ Research workflow

### 1. Search the processed spectra

The first MSFragger search identified candidate peptide matches and assessed peptide identity, PTM identity, and PTM localization. The original project summary reports outcomes across approximately 78,000 processed spectra.

### 2. Construct randomized peptide candidates

The project reports **136,930 distinct peptides** after deduplicating candidates from the top five database hits. Randomized sequences were generated using four operations:

- Permutation without replacement.
- Substitution of an amino acid at a random position.
- Deletion at a random position.
- Insertion of an amino acid at a random position.

For each spectrum, **1,000 randomized peptides** were selected from candidates generated from its top-five hits for the intended second search.

![Four sequence randomization operations illustrated using an invented peptide sequence](assets/ms-thesis/randomization_operators.png)

*The example sequences are invented to explain the operations. Sequence randomization is distinct from adding a biological PTM.*

<details>
<summary>Watch the sequence randomization walkthrough</summary>

![Animation highlighting permutation, substitution, deletion, and insertion on a synthetic peptide sequence](assets/ms-thesis/randomization_walkthrough.gif)

[MP4 version](assets/ms-thesis/randomization_walkthrough.mp4) · [Static version](assets/ms-thesis/randomization_walkthrough_poster.png)

</details>

### 3. Record the second-search limitation

The project summary states that MSFragger could not be run against the custom randomized database because of a tool limitation. This stage should therefore not be interpreted as a completed second MSFragger search.

### 4. Report the GPD experiments separately

The same summary reports 12 GPD runs using two parameter-estimation approaches and two goodness-of-fit approaches. It does not specify the score source linking these runs to the blocked second search. The workflow diagram keeps that connection explicitly unresolved rather than implying that the complete pipeline was executed.

<a id="gpd"></a>

## 📈 What the GPD model explains

A GPD can model threshold exceedances in a score distribution. In the high-score example shown below, a threshold separates the upper tail from the main body of randomized scores. The tail model helps assess how extreme an observed score is relative to that reference distribution.

![Synthetic score distribution with a tail threshold, an illustrative GPD tail density, and the mass beyond an example observed score](assets/ms-thesis/gpd_tail_concept.png)

*Teaching diagram, not an experimental result. A full-null tail probability includes the fraction of scores above the threshold; a conditional tail probability alone is not the complete p-value. The score direction, null construction, threshold selection, and fitting procedure must be specified for an actual analysis.*

The thesis summary lists the following methods:

| Role | Methods reported on the original page |
| --- | --- |
| Parameter estimation | Maximum Likelihood (ML); Probability Weighted Moments (PWM) |
| Goodness-of-fit assessment | Projected Covariance Measure (PCM); Parameter Estimation Anderson Darling (PAD) |
| Number of GPD runs | 12 |
| Recommendation reported by the project | PWM for parameter estimation and PCM for goodness-of-fit assessment |

The reported recommendation is preserved as a project finding. The public repository does not provide per-run comparisons or diagnostic plots, so this page does not add new effect sizes, statistical tests, or claims of calibrated p-values. The names PCM and PAD are retained as given in the source page.

For the generic GPD distribution and survival-function convention, see the [SciPy reference](https://docs.scipy.org/doc/scipy-1.11.0/reference/generated/scipy.stats.genpareto.html). This reference explains the distribution; it is not evidence for the thesis results.

<a id="results"></a>

## 📊 Reported search results

![Reported first-search percentages comparing peptide matches among the top five candidates with exact peptide matches at rank one](assets/ms-thesis/reported_search_results.png)

| Outcome | Result stated in the original summary |
| --- | ---: |
| Peptide match among the top five hits | Approximately 70% of the processed spectra |
| Exact peptide match at rank one | Approximately 58% of the processed spectra |
| Rank-one modified-peptide hits with correct PTM identity and localization | Approximately 29,000 |
| Additional correct rank-one unmodified-peptide hits | Approximately 11,000 |

These outcomes use different matching criteria. The 70% and 58% values describe the initial search; they are not GPD goodness-of-fit or p-value calibration scores. The rounded counts are retained as reported rather than converted into new percentages.

### Reading the C0 to C7 categories

The original result figure classifies rank-one hits using three correctness indicators. Here, **PEP means peptide identity**, not posterior error probability.

![Matrix defining all eight C0 to C7 combinations of peptide identity, PTM identity, and PTM localization correctness](assets/ms-thesis/correctness_categories.png)

| Category | Peptide identity | PTM identity | PTM location |
| --- | --- | --- | --- |
| C0 | Correct | Correct | Correct |
| C1 | Incorrect | Incorrect | Incorrect |
| C2 | Incorrect | Incorrect | Correct |
| C3 | Incorrect | Correct | Correct |
| C4 | Correct | Incorrect | Incorrect |
| C5 | Correct | Correct | Incorrect |
| C6 | Incorrect | Correct | Incorrect |
| C7 | Correct | Incorrect | Correct |

The project treats **C0 as true positive** under its combined correctness definition. For unmodified peptides, the handling of PTM identity and localization needs an explicit convention; the original summary does not specify it.

[View the original detailed MSFragger results](assets/MSFragger%20Results%20Summary.png) · [Download the category definitions as CSV](docs/correctness_categories.csv)

## Project status and reproducibility

This repository presents the research narrative and historical results. It does not currently include a runnable analysis pipeline, search configuration files, the 12 GPD run definitions, or their score-level outputs.

The original summary describes follow-on research into other database-search tools. Further results are not included in the current page. The [research notes](docs/research_notes.md) identify the information needed to reproduce the analysis and distinguish reported findings from the new teaching illustrations.

<a id="thesis-citation"></a>

## 🎓 Thesis citation

A direct link to the thesis and its full bibliographic details will be added here when available. The repository heading describes the research topic and should not be assumed to be the thesis's registered title.

<!-- OWNER EDIT: Replace the paragraph above with the verified thesis citation and permanent university repository URL. Confirm author, registered title, year, degree, and institution. See docs/thesis_citation_template.bib.txt. Do not publish unfilled template fields as a completed reference. -->

## 📁 Repository guide

| Path | Purpose |
| --- | --- |
| `README.md` | Research overview, method explanation, and reported findings |
| `assets/` | Original thesis-summary figures |
| `assets/ms-thesis/` | New explanatory figures, animations, and static posters |
| `docs/research_notes.md` | Evidence boundaries and reproducibility details |
| `docs/correctness_categories.csv` | Searchable C0 to C7 definitions |
| `docs/thesis_citation_template.bib.txt` | Editable citation template to complete later |

No license badge has been added because the inspected repository does not contain a license file. A code or content license can be selected separately by the repository owner.
