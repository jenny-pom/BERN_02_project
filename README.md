# BERN02 Project: Data Extrapolation & Modelling of Plant Data
**Course:** BERN02 Lund University  
**Name:** Jenny Laberg  
**Date:** October 2026  

--- 

## Overview & Objectives

This project builds upon previous work using data from a 2025 intensive survey tracking individual *Antirrhinum majus* plants. The raw data was cleaned during a summer internship 2026 and followed by a preliminary correlation analysis. An efficiency score for each unique `PlantID` was calculated. The analysis then looked at the relationship of the efficiency score with a few phenotypic traits. The primary goal of this project is to expand that baseline analysis by extrapolating spatial and temporal environmental covariates to better understand the drivers of plant reproductive performance. I also aim to expand the models using interactions between phenotypic traits and microclimate (Trait × Environment). 

---

## Dataset Overview & Provenance

* **Source:** The Nick Barton Group at ISTA (Institute of Science and Technology Austria)
* **License:** Proprietary / Restricted Access (Property of the Nick Barton Group; non-public research data)
* **Format:** Comma-Separated Values (`.csv`), UTF-8 encoded
* **Dimensions:** 4,359 rows × 22 columns
* **Missing Value Encoding:** Represented as `NaN` or empty strings (no arbitrary numbers such as `-999`)
* **Primary Key:** `PlantID` (uniquely identifies each observation row)
* **File Locations:**
  * Raw Data: `data/raw/07V_merged_flower_data.csv`
  * Processed Data: `data/processed/07V_processed_flower_data.csv`
### Data Dictionary

| Column Name | Data Type | Units / Scale | Description | Example |
| :--- | :--- | :--- | :--- | :--- |
| `PlantID` | String | Categorical ID | Unique plant identifier | `"PE0008"` |
| `Date` | Date | YYYY-MM-DD | Date of field data collection | `2025-05-26` |
| `Estim.Flowers.Nr` | Integer | Count | Estimated total number of flowers | `14` |
| `Estim.Fruit.Nr` | Integer | Count | Estimated total number of fruits | `0` |
| `Flower.colour` | String | Categorical | Phenotypic trait (e.g., Magenta, Yellow, Hybrid) | `"Magenta"` |
| `Latitude.y` | Float | Decimal Degrees | WGS84 Latitude coordinate of plant | `42.322077` |
| `Longitude.y` | Float | Decimal Degrees | WGS84 Longitude coordinate of plant | `2.072570` |
| `Years_Alive_Count` | Integer | Count | Perenniality (number of years plant has survived) | `2` |

---
## Workflow & Methodological Pipeline

### 1. Spatial Data Augmentation
* **Hydrographic Proximity:** Use `geopandas`, `shapely`, and `osmnx` to compute Euclidean/geodesic distance between plant coordinates and nearest spatial water features.
* **Microclimate Extraction:** Extract topographic attributes (elevation, slope, aspect, canopy cover) to serve as environmental covariates.

### 2. Statistical Modeling of Reproductive Efficiency
* **Target Variable:** Reproductive Efficiency Ratio ($\text{Max Fruits} / \text{Total Flowers}$).
* **Models Evaluated:**
  * **Baseline Model:** Ordinary Least Squares (OLS) Linear Regression.
  * **Binomial GLM (Logistic Regression):** Success counts (`Max Fruits`) vs. failure counts (`Total Flowers - Max Fruits`).
  * **Beta Regression & GLMMs:** Continuous proportion bounds $(0, 1)$ and Generalized Linear Mixed Models (GLMMs) accounting for spatial transect random effects.
* **Predictors:** Distance to water, microclimate variables, flower color phenotypes, transect locations, and cumulative seasonal flower production.

### 3. Data Visualization & Web Application
* **Interactive Mapping:** Web-based spatial map built with `Folium` and `Plotly` displaying plant locations, efficiency scores, color traits, and water proximity.

## Environment Setup & Reproducibility

### Prerequisites
* Python 3.11+
* [`uv`](https://github.com/astral-sh/uv) package manager (recommended) or standard `pip`

### Clone Repository
```bash
git clone https://github.com/jenny-pom/BERN_02_project.git
cd BERN02_project
```


## Repository Structure
```text
BERN02_project/
├── README.md
├── LICENSE
├── pyproject.toml
├── requirements.txt
├── data/
│   ├── raw/
│   └── processed/
└── notebooks/
    ├── processed_data_analysis.ipynb (previous work)
    └── main_analysis.ipynb
