# BERN02 Project: Data Extrapolation & Modelling of Plant Data
**Course:** BERN02 Lund University  
**Name:** Jenny Laberg  
**Date:** October 2026  

--- 

## Overview & Objectives

This project builds upon a previous data collection from an intensive survey tracking individual *Antirrhinum majus* plants. The raw data was collected 2025 and cleaned during a summer internship 2026. An efficiency score for each unique `PlantID` was calculated to filter plants based on they reproductive success. The primary goal of this project is to expand the set of biological predictors with abiotic predictors through extrapolatio. I will then use modeling to see what features potentially drive plant reproductive sucees. 

---

## Dataset Overview & Provenance

* **Source:** The Nick Barton Group at ISTA (Institute of Science and Technology Austria)
* **License:** Proprietary / Restricted Access (Property of the Nick Barton Group; non-public research data)
* **Format:** Comma-Separated Values (`.csv`), UTF-8 encoded
* **Dimensions:** 4,359 rows × 22 columns
* **Missing Value Encoding:** Represented as `NaN` or empty strings (no arbitrary numbers such as `-999`)
* **Primary Key:** `PlantID` (uniquely identifies each observation row)

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
| `dist_to_river_m` | Float | Meters | Distance to nearest river or watercourse | `90.785869` |
| `elevation_m` | Float | Meters | Elevation above mean sea level derived from DEM | `1155.040039` |
| `slope_deg` | Float | Degrees | Terrain steepness angle | `47.657982` |
| `northness` | Float | Index (-1 to 1) | Aspect orientation relative to North (cos(aspect)) | `-0.419594` |
| `tri_ruggedness` | Float | Index | Terrain Ruggedness Index (local elevation heterogeneity) | `0.471634` |
| `ndvi` | Float | Index (-1 to 1) | Normalized Difference Vegetation Index (canopy greenness) | `0.164706` |

---
## Workflow & Methodological Pipeline
### 1. Data Visualisation 
Plot the biolocal features in relationship to plant success. Lot for correlation, variance of data, remove outliers if necessary etc. 

### 2. Data Augmentation
Use `geopandas`, `rasterio`and `fiona` to handle spatial operations on geometric types and reading and handling geospatial raster datasets and vector spatial data files. 


### 3. Statistical Modeling of Reproductive Efficiency
Target Variable: Reproductive Efficiency Ratio ($\text{Max Fruits} / \text{Total Flowers}$).

I will use spearman rank correlation (scipy.stats.spearmanr) to evaluate monotonic relationships across non-normally distributed counts (e.g., flower production vs. fruit yield).

I will use OLS regression (statsmodels.api.OLS and scipy.stats.linregress) to assess predictors of reproductive success and scrren for multicollinearity using variance inflation factor (variance_inflation_factor) to eliminate collinear environmental and spatial predictors prior to fitting final linear models.

### 4. Data Visualization & Web Application
I will use Seaborn and Matplotlib to visualize phenotype comparisons and geographic maps.

## Environment Setup & Reproducibility

### Prerequisites
* Python 3.11+
* [`uv`](https://github.com/astral-sh/uv) package manager

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
