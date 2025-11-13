# Predicting Agricultural Statistics for Texas Counties Using Climate Data

## Teammates

- Carter Dobbs (dgi6)
- Johann Steinhoff (ngq7)
- Jay Suh (hkm55)

## Project Abstract

We will build a unified regression model that predicts agricultural outcomes (yield, production, harvested acreage, sales) for Texas counties based on climate conditions, crop types, and temporal patterns. Using 398,204 records merging USDA agricultural data with NOAA climate data (47.8M floats total), we'll implement a Decision Tree baseline, improve it with AdaBoost, and apply PCA to address the curse of dimensionality from 72+ features.

## Problem Statement

**Problem:** Climate variability significantly impacts agriculture, but predicting specific agricultural outcomes from weather patterns across diverse crops and regions remains challenging. This project addresses: How accurately can we predict agricultural statistics (yield, production, area) for Texas counties using climate data?

**Importance:** Texas agriculture represents a $100B+ industry. Understanding climate-productivity relationships informs drought preparedness, resource allocation, and economic planning.

**Benchmark:** We use three baselines: (1) mean prediction per crop/statistic, (2) persistence model (previous year's value), and (3) temporal Decision Tree regression. No existing benchmark combines this specific data fusion.

**Data Sources:**

- USDA NASS QuickStats: 398,204 Texas county-level records (2000-2023), 165 crops, 16 statistic types
- NOAA nClimDiv: Monthly climate data for 255 counties (precipitation, temperature, degree days)

**Success Measures:**

- R² > 0.6 on held-out test set (2020-2023)
- Model predicts lower yields during known drought years (2011, 2013-2014)
- Climate features rank high in feature importance
- AdaBoost improves baseline RMSE by 10-20%

**Hope to Achieve:** We hope to achieve a successful model that can accurately predict agricultural product outcomes based on environmental fluctuations. These fluctuations can be anywhere from rainfall, to drought levels as well as duration for each. With this predictive model we can hopefully have an impact on farmers within Texas counties and assist them.

## Dataset

**Size:** 398,204 records × 120 features = **47,784,480 floats** (exceeds 10M requirement)

**Sources:**

- USDA QuickStats (filtered from 23.3M nationwide records, 7GB)
- NOAA nClimDiv county-level climate division data (28 files, fixed-width format)

**Format:** Merged CSV (850 MB) combining agricultural statistics with climate variables via county FIPS codes and year. 96.5% merge success rate.

**Key Attributes (72 total):**

- *Categorical:* County (255), Crop (165), Statistic Type (16), Year (24)
- *Climate (60):* Monthly precipitation, max/min/avg temperature, degree days (Jan-Dec)
- *Engineered (8):* Growing season precipitation/temperature (April-Sept), annual aggregates
- *Target:* VALUE (range: 0.1 to 2.8B depending on statistic type and unit)

**Suitability:** Multi-source fusion creates novel dataset linking government agricultural records with authoritative climate data. Temporal span (24 years) includes major drought events for validation. County-level granularity balances detail with data availability.

## Methodology

**Pre-training objectives:**

- Going through dataset to find potentially useless data that may impact the training and predictive process
- Using tools such as powerBI and other query tools to cleanse and organize data to reduce variance and outliers

**Baseline:** Decision Tree Regressor

- Handles mixed categorical/numerical data naturally
- Captures non-linear climate-agriculture relationships
- Provides interpretable feature importance

**Improvement:** AdaBoost Ensemble

- Combines multiple weak learners to reduce bias and variance
- Expected 10-20% RMSE improvement over baseline
- More robust to outliers in agricultural data

**Dimensionality Analysis:** Principal Component Analysis (PCA)

- Address curse of dimensionality from 400+ features after encoding
- Compare model performance with/without dimensionality reduction
- Analyze trade-offs between accuracy and interpretability

**Evaluation:** Temporal train/validation/test split (2000-2018 / 2019 / 2020-2023). Metrics: R², RMSE (normalized per statistic type), MAE.

## Teaming Strategy

**Meeting Schedule:** Weekly video calls (Sundays 6 PM) + asynchronous collaboration via Discord/GitHub

**Timeline:**

- Week 1-2: Data preprocessing, missing value handling, feature encoding
- Week 3: Baseline Decision Tree implementation and evaluation
- Week 4: AdaBoost improvement and hyperparameter tuning
- Week 5: PCA analysis and dimensionality experiments
- Week 6: Final evaluation, visualizations, report writing

**Tools:** Jupyter notebooks (version control via Git), scikit-learn, pandas, shared Google Drive for documentation

**Success Mechanisms:** Weekly progress check-ins, shared task board with deadlines, pair programming for complex sections

### Role Assignments and Commitment Matrix

*(To be completed with team assignments - example structure below)*

- Data preprocessing & feature engineering: TBD
- Baseline model implementation: TBD
- AdaBoost improvement: TBD
- PCA analysis: TBD
- Evaluation & visualization: TBD
- Report writing & documentation: All team members

## Mitigation Plan

**Data Alternative:** If climate merge fails, use only temporal/spatial features (county, year, crop) as baseline predictors. Alternative: PRISM climate data or Texas specific weather station data.

**Method Alternative:** If Decision Tree performs poorly (R² < 0.3), switch to Random Forest baseline. If AdaBoost doesn't improve, try Gradient Boosting or Neural Network.

**Teammate MIA:** Redistribute tasks among remaining members with extended timeline. Critical paths (data prep, baseline model) assigned to most reliable members first.

**GIGO Scenario:** If baseline R² < 0.2, investigate: (1) Need for log transformation of target variable, (2) Separate models per statistic type rather than unified approach, (3) Focus on subset of high-quality crops (major commodities only), (4) Add more features (soil type, irrigation data if available).
