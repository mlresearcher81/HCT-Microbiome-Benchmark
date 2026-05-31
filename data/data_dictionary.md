# HCT-Microbiome Benchmark: Data Dictionary

This document outlines the schema, provenance, and definitions for the clinical and taxonomic features provided in the `HCT_Microbiome_Benchmark.csv` file. 

The dataset is initially formatted in a "long" observational structure, which is subsequently pivoted into a wide feature tensor ($X \in \mathbb{R}^{5 \times 119}$) by the pipeline's preprocessing module.

---

## 1. Dataset Provenance & Curation Methodology

**Original Data Source:**
The raw 16S rRNA gene sequencing data and corresponding clinical hospitalome metadata utilized to construct this benchmark were derived from the publicly available, multi-center allogeneic hematopoietic cell transplantation (HCT) cohort published by Liao et al. (2021).

**Curation and Preprocessing Steps:**
To transform the sporadic observational archive into the strictly formatted temporal benchmark provided in this repository, the following curation pipeline was programmatically applied:
1. **Temporal Alignment:** A continuous clinical timeline ranging from Day -15 to Day +35 relative to the HCT event was generated for each of the 1,870 patients. 
2. **Clinical Imputation:** Missing continuous clinical vitals (e.g., Maximum Daily Temperature, Neutrophil Counts) were resolved using forward-filling (`ffill`) to preserve state persistence between sporadic clinical measurements.
3. **Taxonomic Aggregation:** Amplicon Sequence Variants (ASVs) were mapped using the SILVA taxonomy and mathematically aggregated at the Genus level. Relative abundances were calculated natively per sample.
4. **Structural Zero-Filling:** Days featuring clinical data but lacking a corresponding stool sample (or a sample missing specific taxa) were zero-filled, accurately reflecting the 98.04% biological absence/limit of detection intrinsic to the cohort.
5. **Phase II Variance Filtration:** Genera exhibiting a cross-sample variance of $\sigma^2_v < 10^{-6}$ after a $\log(1+x)$ transformation were computationally pruned, isolating the 119 most dynamically active taxonomic features.

---

## 2. Index & Metadata Variables

| Column Name | Data Type | Description | Example |
| :--- | :--- | :--- | :--- |
| **PatientID** | String / Categorical | Unique anonymized identifier for each HCT patient. Used to strictly enforce Group Shuffle Splitting to prevent temporal leakage across train/test splits. | `Patient_1012` |
| **SampleID** | String / Categorical | Unique identifier for a specific 16S rRNA fecal sequencing sample. | `Sample_4492` |
| **DayRelativeToNearestHCT** | Integer | The day the sample was collected relative to the time of transplantation. Negative values indicate pre-transplant conditioning; positive values indicate post-transplant recovery. | `-5`, `0`, `14` |

## 3. Clinical Target Variable

| Column Name | Data Type | Description | Example |
| :--- | :--- | :--- | :--- |
| **Dysbiosis_Label** | Integer (Binary) | The supervised target variable representing impending ecological collapse within the next 48 hours. `1` indicates imminent dysbiosis; `0` indicates a stable ecological state. | `0` or `1` |

## 4. Taxonomic Features (Microbiome Telemetry)

After Phase II variance filtration, the feature space contains **119 Active Genera**. 

*Note: In the raw CSV format, these are represented as rows (long format) identifying the Genus and its Relative Abundance. During Phase I preprocessing in the benchmark notebook, these are pivoted into 119 distinct columns aligned to the continuous 5-day sampling windows.*

| Column Name | Data Type | Description | Range |
| :--- | :--- | :--- | :--- |
| **Genus** | String / Categorical | The taxonomic classification of the microbe aggregated at the Genus level (e.g., *Bacteroides*, *Enterococcus*, *Lactobacillus*). | `Bacteroides` |
| **RelativeAbundance** | Float | The untransformed, compositional relative abundance of the specified Genus in that specific daily sample. Represents the proportion of the total microbial community. | `0.0` to `1.0` |

## 5. Derived Ecological Metrics (Calculated Natively)

While not explicitly provided in the raw CSV to prevent data leakage, the following ecological metrics are calculated natively by the Python pipeline during preprocessing and temporal trajectory analysis:

* **Shannon Diversity Index:** Calculated using the natural logarithm of the relative abundances to track overall ecosystem complexity and decline.
* **Max Genus Domination:** The relative abundance of the single most dominant Genus in a sample, used to define the ecological collapse boundary ($\ge 30\%$).