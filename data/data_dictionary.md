# HCT-Microbiome Benchmark: Data Dictionary

This document outlines the schema and definitions for the clinical and taxonomic features provided in the `HCT_Microbiome_Benchmark.csv` file. 

The dataset is formatted in a "long" observational structure, which is subsequently pivoted into a wide feature matrix ($X \in \mathbb{R}^{5 \times 119}$) by the preprocessing pipeline.

## 1. Index & Metadata Variables

| Column Name | Data Type | Description | Example |
| :--- | :--- | :--- | :--- |
| **PatientID** | String / Categorical | Unique anonymized identifier for each hematopoietic cell transplantation (HCT) patient. Used to strictly enforce Group Shuffle Splitting to prevent temporal leakage. | `Patient_1012` |
| **SampleID** | String / Categorical | Unique identifier for a specific 16S rRNA fecal sequencing sample. | `Sample_4492` |
| **DayRelativeToNearestHCT** | Integer | The day the sample was collected relative to the time of transplantation. Negative values indicate pre-transplant conditioning; positive values indicate post-transplant recovery. | `-5`, `0`, `14` |

## 2. Clinical Target Variable

| Column Name | Data Type | Description | Example |
| :--- | :--- | :--- | :--- |
| **Dysbiosis_Label** | Integer (Binary) | The supervised target variable representing impending ecological collapse within the next 48 hours. `1` indicates imminent dysbiosis; `0` indicates a stable ecological state. | `0` or `1` |

## 3. Taxonomic Features (Microbiome Telemetry)

After Phase-II variance filtration ($\sigma^2_v > 10^{-6}$), the feature space contains **119 Active Genera**. 

*Note: In the raw CSV format, these are represented as rows (long format) identifying the Genus and its Relative Abundance. During Phase-I preprocessing, these are pivoted into 119 distinct columns aligned to the daily sampling windows.*

| Column Name | Data Type | Description | Range |
| :--- | :--- | :--- | :--- |
| **Genus** | String / Categorical | The taxonomic classification of the microbe aggregated at the Genus level (e.g., *Bacteroides*, *Enterococcus*, *Lactobacillus*). | `Bacteroides` |
| **RelativeAbundance** | Float | The untransformed, compositional relative abundance of the specified Genus in that specific daily sample. Represents the proportion of the total microbial community. | `0.0` to `1.0` |

## 4. Derived Ecological Metrics (Calculated Natively)

While not explicitly provided in the raw CSV to prevent data leakage, the following ecological metrics are calculated natively by the Python pipeline during preprocessing and temporal trajectory analysis:

* **Shannon Diversity Index:** Calculated using the natural logarithm of the relative abundances to track overall ecosystem complexity and decline.
* **Max Genus Domination:** The relative abundance of the single most dominant Genus in a sample, used to define the ecological collapse boundary.