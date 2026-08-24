<picture>
  <source media="(prefers-color-scheme: dark)" srcset="assets/header-dark.svg">
  <img alt="Xiaoyu Zong, Healthcare Data Scientist. Biobank and multimodal health data. The analysis pipelines a research group runs on, and the last review before it publishes." src="assets/header-light.svg">
</picture>

[![Open to](https://img.shields.io/badge/Open%20to-Data%20Scientist%20·%20RWE%20Scientist%20·%20Research%20Scientist-0969da?style=flat-square)](https://www.linkedin.com/in/xiaoyu-zong-0a733ba0)
![Location](https://img.shields.io/badge/St.%20Louis%2C%20MO-open%20to%20relocation%20%26%20remote-59636e?style=flat-square)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-connect-0a66c2?style=flat-square&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/xiaoyu-zong-0a733ba0)
[![ORCID](https://img.shields.io/badge/ORCID-0000--0001--5646--710X-a6ce39?style=flat-square&logo=orcid&logoColor=white)](https://orcid.org/0000-0001-5646-710X)

**Open to Data Scientist, Real-World Evidence Scientist and industry Research Scientist
roles in health tech and pharma.**
Based in St. Louis, MO; open to relocation and remote.

I own the analysis pipelines and templates a research group at Washington University
School of Medicine runs on, and I manage its data. Eighteen researchers have run their
analyses through them: two assistant professors, a postdoc, three PhD students, five MPH
research assistants, two clinical fellows, two residents and three visiting scholars. I
write the statistical methods sections, and often help design the analyses they describe.
The last review before submission is mine, and what I am accountable for is that the
numbers are right.

- **Cited in NCCN guidelines.** Lead data analyst on the *JNCI* 2023 red-flag signs and
  symptoms study, a matched case-control analysis of 5,075 cases drawn from **113 million US
  commercial claims beneficiaries**, which the **NCCN Colorectal Cancer Screening
  Guidelines** cite (v1.2024 through v2.2026) for their recommendation on evaluating alarm
  signs and symptoms. The only other reference for that recommendation is a meta-analysis
  that pools it.
- **27 peer-reviewed papers, 1,300+ citations, h-index 17**, co-first on three, in venues
  including *Nature Medicine*, *Nature Genetics*, *Cell Metabolism* and *JAMA Oncology*.
- **Extraction, QC and processing I built myself** across commercial claims (MarketScan),
  Medicare, UK Biobank, All of Us, Our Future Health, NHANES, the Harvard cohorts, ALSPAC,
  SEER and CDC WONDER, much of it inside
  controlled-access environments, in R, Python, SQL and SAS.

## Software

**[JM-omp](https://github.com/j262byuu/JM-omp)**<br>
Rcpp/OpenMP rewrite of the `JM` R package for joint longitudinal-survival models, built
because upstream `JM` was too slow to fit a full-size NHS II cohort. Same API; at 100,000
subjects the Weibull-PH fit matches CRAN `JM`'s log-likelihood to the last printed digit,
same iteration count, same convergence flag. The
[benchmark table](https://github.com/j262byuu/JM-omp#speedup) is on simulated cohorts, and
the repo states which methods are not accelerated and which two columns are measured against
different baselines. A collaborator is fitting a real NHS II cohort with it now; I will
quote real-cohort figures here once that lands.

**[GGIR performance work](https://github.com/j262byuu/GGIR/branches/all)**<br>
[Officially listed contributor](https://wadpac.github.io/GGIR/authors.html) on GGIR, the
standard accelerometry toolkit, with three defects filed upstream, all three fixed and
credited in GGIR's release notes. 15 further correctness and performance branches, each
benchmarked in isolation with equivalence verified at the output; one submitted, the rest
queued
([roadmap](https://github.com/j262byuu/docker-accelerometer#performance-optimization-roadmap)).
Per-branch measurements live in that roadmap and are not quoted here until they are merged.

**[docker-accelerometer](https://github.com/j262byuu/docker-accelerometer)**<br>
Reproducible GGIR stack for Docker, Apptainer and HPC. Image reduced **4.34 GB to 1.92 GB**,
thread-capped so a scheduler allocation is never silently oversubscribed.

**[svyrcs](https://github.com/j262byuu/svyrcs)**<br>
Restricted cubic splines fitted *inside* a complex survey design. Closes **8 documented
ways** a hand-rolled script gets this wrong: Barnard-Rubin df, design-based *F*, *t*
quantiles, weighted knots. CI, tests, vignette, MIT.

More: **[nhanes-accel-1114](https://github.com/j262byuu/nhanes-accel-1114)** and
**[0306](https://github.com/j262byuu/nhanes-accel-0306)**, NHANES accelerometry through a
shared GGIR workflow, validated against the NCI SAS reference (97.1% MVPA, adults,
unweighted) with comparability limits documented rather than glossed ·
**[gcta](https://github.com/j262byuu/gcta)**, COJO variance-ratio filter removed so
large-effect pQTLs survive joint analysis.

## How I benchmark

Both arms built from the same source and run back to back in one job on one physical host;
seven replicates over ten UK Biobank recordings; a branch only counts if it wins every
pair. Equivalence is verified at the output rather than assumed, and the same reading
caught four defects upstream: three in GGIR, and systematic wake
overcalling in [`asleep`](https://github.com/OxWearables/asleep/issues/66) on
Idle-Sleep-Mode NHANES recordings, filed with diagnostics, a GGIR comparison and a
proposed flag.

I document negative results too. Intel MKL looked like a clear win and was removed from
`docker-accelerometer` after measurement showed it indistinguishable from OpenBLAS at the
one thread the image actually uses, while costing 2.4 GB and [one hard crash
mode](https://github.com/j262byuu/docker-accelerometer#phase-1-intel-mkl--tried-measured-removed).
That README carries a section called *What is deliberately not claimed*. I would rather
publish the negative result than carry a number I cannot defend.

## Work I cannot show

The data cannot leave. The methods can.

The pipelines my group runs on live in private repositories, inside controlled-access
perimeters: All of Us Controlled Tier, DNAnexus RAP, a health system's OMOP data lake, a
cohort cluster holding Controlled Unclassified Information.

- **EHR phenotyping on OMOP** (SQL on Databricks): concept-set definitions carrying the literal rule, the
  coverage it produced, and an orphan review of what the rule *missed*. An inventory of
  what a definition caught cannot show you an omission.
- **Polygenic risk scores at cohort scale** (Hail, BigQuery): WGS against array, genome build and allele
  harmonisation, missingness, variant annotation.
- **Multi-cohort harmonisation:** pooling the Harvard cohorts, which run on SAS, through
  variable-level coverage matrices and impossible-entry worksheets a human signs off before
  anything is combined.
- **Wearable phenotypes:** multilevel functional PCA over one-minute actigraphy in three
  cohorts, cosinor and sleep-regularity metrics, physiological circadian modelling.
- **Observational-design hygiene:** landmark anchoring, multiple imputation with Rubin
  pooling, negative-control outcomes and empirical calibration.
- **Review as a deliverable:** verification passes that re-measure numbers already
  reported, defect-class audits, independent second-reader code review before sign-off.
- **Independent replication** of a published *JAMA Network Open* analysis, reproducing its
  estimates from the source data.

Each repository carries an explicit data red line, and a `.gitignore` that keeps
controlled-data paths out of Git tracking. The real control is checking what you are about
to commit.

## Research

**Co-first author on three papers:** *Gut* (2021), metabolic syndrome and early-onset
colorectal cancer (**260 citations**, the most-cited in the record); *Journal of Clinical
Oncology* (2023), clonal hematopoiesis and incident lung cancer; *Hepatology
Communications* (2022), fatty liver disease and gastrointestinal cancers. Second author on
*Nature Medicine* (2026).

Full record on [ORCID](https://orcid.org/0000-0001-5646-710X).

## Harness engineering

The analysis behind most of those papers predates LLM coding assistants. I build and run
coding harnesses now, and they raise throughput without moving the standard: generated code
passes the same review, benchmark and equivalence gates as a colleague's, and the final
sign-off remains mine.

## Stack

**Languages:** R · C++ (Rcpp/OpenMP) · Python · SQL · SAS · Bash

**Platforms:** All of Us Researcher Workbench / Verily · DNAnexus RAP · Databricks · BigQuery · Hail · Docker · Apptainer/Singularity · LSF · Slurm · GitHub Actions

**Cohorts & data systems:** UK Biobank · All of Us · Our Future Health · NHANES · NHIS · Nurses' Health Study I/II/3 · HPFS · GUTS I/II · ALSPAC (Avon) · SEER · CDC WONDER

**Data:** OMOP CDM · EHR (Epic Clarity ETL) · proteomics (Olink) · genomics · microbiome (shotgun metagenomics via bioBakery, 16S via DADA2) · accelerometry · complex survey data

**Methods:** survival analysis · joint longitudinal–survival models · propensity scores · Mendelian randomization and colocalization · multiple imputation with Rubin pooling · negative-control outcomes and empirical calibration · polygenic risk scores · multilevel functional PCA · survey-weighted regression · restricted cubic splines · statistical genetics

## Contact

[LinkedIn](https://www.linkedin.com/in/xiaoyu-zong-0a733ba0) ·
[ORCID](https://orcid.org/0000-0001-5646-710X)
