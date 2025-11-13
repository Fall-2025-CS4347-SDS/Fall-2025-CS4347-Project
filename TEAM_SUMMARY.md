# Project Summary: Predicting Texas Agricultural Productivity Using Climate Data

## Quick Overview

**Project Title:** Predicting Agricultural Statistics for Texas Counties Using Climate Data

**Goal:** Build a machine learning model that predicts agricultural outcomes (crop yields, production volumes, harvested acreage, sales) for Texas counties based on climate conditions and crop types.

**Why This Matters:** Climate variability significantly impacts agriculture. This project helps understand and predict how weather patterns affect different aspects of farming productivity across Texas counties, which can inform drought preparedness, resource allocation, and economic planning.

## The Dataset

### What We Have

**1. USDA NASS Agricultural Data**
- Source: U.S. Department of Agriculture QuickStats database
- Size: Started with 23.3 million records nationwide (7GB file)
- Texas Subset: 398,204 records covering:
  - 255 counties
  - 24 years (2000-2023)
  - 165 different crops (cotton, wheat, corn, sorghum, hay, vegetables, fruits, nuts, etc.)
  - 16 statistic types (yield, production, area harvested, area planted, sales, price, etc.)

**2. NOAA Climate Data**
- Source: National Oceanic and Atmospheric Administration (nClimDiv county-level data)
- Coverage: Monthly climate data for all Texas counties, 2000-2024
- Variables:
  - Precipitation (monthly rainfall in inches)
  - Temperature (max, min, average in °F)
  - Cooling/Heating Degree Days
  - Engineered features: Growing season aggregates (April-September totals/averages)

**3. Final Merged Dataset**
- **398,204 records × 120 features = 47,784,480 data points** (exceeds 10M requirement)
- 96.5% merge success rate (384,071 records have complete climate data)
- Output file: `texas_agriculture_with_climate_2000_2023.csv` (~850 MB)

### Dataset Characteristics

**Strengths:**
- Large scale: Nearly 400K records with rich feature set
- Multi-dimensional: Spatial (255 counties), temporal (24 years), categorical (165 crops)
- Real-world government data from authoritative sources
- Comprehensive climate coverage with minimal missing data

**Challenges:**
- Sparsity: Not all counties grow all crops (cotton in West TX, rice in coastal areas, etc.)
- Multiple units: Yields measured in BU/ACRE, LB/ACRE, TONS, CWT; sales in dollars
- Missing values: 3.5% of records lack climate data (mostly recent years 2020-2023)
- Privacy suppression: Some small values suppressed by USDA to protect farmer privacy
- Scale differences: Values range from acres (10s-1000s) to production (millions of pounds) to sales (millions of dollars)

## The Machine Learning Task

### Problem Type: Unified Multi-Output Regression

Instead of building separate models for each statistic (one for yield, one for production, etc.), we treat **statistic type as an input feature** and build a single unified model.

**Input Features (~72 features):**
- **Categorical:**
  - County (255 values)
  - Crop type (165 values)
  - Statistic type (16 values: yield, production, area harvested, etc.)
  - Year (24 values: 2000-2023)
  
- **Numerical (60 climate features):**
  - Monthly precipitation (12 values: Jan-Dec)
  - Monthly max temperature (12 values)
  - Monthly min temperature (12 values)
  - Monthly avg temperature (12 values)
  - Monthly degree days (12 values)
  
- **Engineered Features (8 features):**
  - Growing season precipitation (April-September sum)
  - Growing season temperature (avg, max, min)
  - Annual precipitation
  - Annual temperature
  - Annual cooling/heating degree days

**Output:**
- Single numerical value (the VALUE field)
- Interpretation depends on statistic type:
  - If statistic = "YIELD": value is bushels per acre
  - If statistic = "PRODUCTION": value is total bushels
  - If statistic = "AREA HARVESTED": value is acres
  - etc.

### Why This Approach?

**Advantages:**
1. Single model learns shared patterns across all statistics
2. Can predict multiple metrics simultaneously
3. More data-efficient (uses all 398K records instead of splitting into 16 smaller datasets)
4. Captures relationships between different statistics

**Challenge:**
- Different scales require careful evaluation (can't use RMSE directly across all types)

## Machine Learning Methods

### Baseline: Decision Tree Regressor
- Simple, interpretable model
- Can handle non-linear relationships
- Naturally handles mixed categorical/numerical data
- Provides feature importance scores

### Improvement Method: AdaBoost
- Ensemble method that combines multiple weak learners
- Expected to improve predictions by 10-20% over baseline
- More robust to outliers and noise

### Addressing Curse of Dimensionality: PCA
- With 72+ features after encoding, we'll demonstrate dimensionality issues
- Apply Principal Component Analysis (PCA) to reduce dimensions
- Compare performance before/after PCA
- Discuss trade-offs (accuracy vs. interpretability)

## Evaluation Strategy

### Data Splits (Temporal)
- **Training:** 2000-2018 (19 years, ~300K records)
- **Validation:** 2019 (1 year, ~16K records) - for hyperparameter tuning
- **Test:** 2020-2023 (4 years, ~80K records) - held-out final evaluation

Why temporal splits? Ensures model can predict future years (more realistic than random splits)

### Metrics

**Primary Metrics:**
- **R² (Coefficient of Determination):** Target > 0.6
- **RMSE (Root Mean Squared Error):** Normalized by mean for each statistic type
- **MAE (Mean Absolute Error):** Robust to outliers

**Baseline Comparisons:**
1. Mean prediction (predict average value for each crop/statistic combination)
2. Persistence model (predict last year's value)
3. Linear trend (simple year-based regression)

**Success Validation:**
- Model should predict lower yields during known drought years (2011, 2013-2014)
- Feature importance should highlight climate variables
- Geographic patterns should match known agricultural regions

## Project Status

### Completed
- [x] Downloaded raw USDA NASS data (23.3M records)
- [x] Downloaded NOAA climate data (28 files)
- [x] Built complete data preparation pipeline
- [x] Filtered to Texas county-level data (398K records)
- [x] Parsed fixed-width climate files
- [x] Merged agricultural + climate data (96.5% success)
- [x] Created engineered features
- [x] Validated data quality
- [x] Confirmed 47.8M data points (exceeds requirement)
- [x] Created comprehensive documentation

### Next Steps
1. Handle missing values (imputation strategy)
2. Encode categorical variables (one-hot or target encoding)
3. Implement baseline Decision Tree model
4. Implement AdaBoost improvement
5. Apply PCA and compare results
6. Evaluate on test set
7. Analyze feature importance
8. Document findings

## Technical Details

### File Structure
```
project/
├── source-data/
│   ├── qs.crops_20251108.txt          # Raw NASS data (7GB)
│   ├── climdiv-*.txt                  # NOAA climate files (28 files)
│   └── county-to-climdivs.txt         # County mapping
├── data_preparation.ipynb              # Complete data pipeline
├── texas_agriculture_with_climate_2000_2023.csv  # Final dataset (850MB)
├── PROJECT_PROPOSAL_SUBMISSION.md      # Formal proposal
└── PROJECT_PROPOSAL.md                 # Technical documentation
```

### Data Pipeline
1. **Load:** Read 23.3M records from USDA QuickStats
2. **Filter:** Extract Texas county-level data (2000-2023)
3. **Parse:** Convert NOAA fixed-width format to structured data
4. **Merge:** Join on (County FIPS, Year)
5. **Engineer:** Create growing season and annual aggregates
6. **Export:** Save combined dataset for modeling

### Requirements Met
- [x] Dataset > 10M floats: **47,784,480 data points**
- [x] Multi-source data: USDA + NOAA + spatial/temporal
- [x] No existing benchmark: Novel data fusion approach
- [x] Decision Tree baseline + AdaBoost improvement
- [x] PCA for curse of dimensionality discussion
- [x] Complete pipeline from raw data to model-ready dataset

## Key Insights (So Far)

1. **Data Quality:** 96.5% merge success indicates good alignment between sources
2. **Temporal Patterns:** Merge success varies by year, with major agricultural census years (2002, 2007, 2012, 2017, 2022) showing highest coverage
3. **Geographic Coverage:** All 255 Texas counties represented, though not all grow all crops
4. **Climate Variability:** Monthly data captures seasonal patterns important for agriculture
5. **Feature Richness:** 120 features provide comprehensive view of agricultural and climate conditions

## Questions for the Team

1. **Scope:** Do we want to focus on specific crops (e.g., major commodities like cotton, wheat, corn) or keep all 165?
2. **Missing data strategy:** Should we impute missing climate values or drop those records?
3. **Feature encoding:** One-hot encoding for categories will create 400+ features. Use target encoding instead?
4. **Evaluation focus:** Which metrics matter most to us? Should we weight certain crops/statistics more heavily?
5. **Interpretability:** How important is model explainability vs. pure predictive performance?
6. **Timeline:** Who takes which modeling tasks? (baseline, AdaBoost, PCA, visualization, report writing)

## Why This Project is Interesting

1. **Real-world impact:** Agriculture is a $100B+ industry in Texas
2. **Climate relevance:** Timely topic given increasing climate variability
3. **Data science skills:** End-to-end pipeline from messy raw data to predictions
4. **Technical challenges:** Large dataset, multi-source fusion, mixed data types
5. **Multiple ML concepts:** Regression, ensemble methods, dimensionality reduction
6. **Novel approach:** Unified modeling strategy not seen in existing literature
7. **Interpretable results:** Can identify which climate factors matter most

## Contact & Collaboration

**Repository:** All code in `data_preparation.ipynb` with complete documentation
**Dataset:** `texas_agriculture_with_climate_2000_2023.csv` ready for modeling
**Proposals:** Both technical (`PROJECT_PROPOSAL.md`) and submission-ready (`PROJECT_PROPOSAL_SUBMISSION.md`) versions available

Let me know your thoughts, questions, or if you'd like to focus on specific aspects of the project!
