# Project Proposal Title

Predicting Texas Agricultural Production Using Climate Data

## Teammates

- Carter Dobbs (dgi6)
- Johann Steinhoff (ngq7)
- Jay Suh (hkm55)

## Project Abstract

We're building a regression model to predict agricultural statistics (yield, production volume, acres harvested) for Texas counties based on climate conditions. Our dataset merges USDA agricultural records with NOAA climate data spanning 2000-2023, covering 254 counties and 165 crop types. We'll start with a Decision Tree baseline and improve it using AdaBoost ensemble methods. The model will help identify which climate factors most strongly influence crop outcomes, with potential applications in drought planning and resource allocation for Texas's $100B agriculture industry.

## Problem Statement

**Problem:** Can we predict agricultural outcomes for Texas counties using climate variables like precipitation, temperature, and growing degree days?

**Importance:** Texas agriculture generates over $100 billion annually. Understanding climate-yield relationships can inform drought preparedness, irrigation planning, and crop selection decisions. Historical patterns show dramatic yield drops during drought years, but we lack quantitative models connecting specific climate conditions to production outcomes.

**Benchmark:** We'll compare three baselines:

1. Simple mean prediction (average yield per crop type)
2. Persistence model (use previous year's value)
3. Decision Tree Regressor (our primary baseline)

The Decision Tree handles mixed data types well and provides interpretable feature importance, making it ideal for understanding climate-agriculture relationships.

**Data sources:**

- USDA NASS QuickStats: 398,204 county-level agricultural records (2000-2023) covering 165 commodities
- NOAA nClimDiv: Monthly climate measurements for 254 Texas counties including precipitation, max/min/avg temperature, and degree days

**Success measures:**

- R² > 0.70 on test set (explains most variance)
- Model correctly predicts yield decreases during known drought years (2011, 2013)
- Climate features appear in top 10 feature importance rankings
- RMSE below persistence model benchmark

**Goal:** Build an interpretable model that quantifies how climate affects Texas crop production, providing actionable insights for agricultural planning.

## Dataset

**Size:** 398,204 rows x 120 features = 47,784,480 data points

**Sources:**

- USDA NASS QuickStats: Filtered from 23.3M nationwide records (7GB raw data)
- NOAA nClimDiv: 28 fixed-width format climate files with county-level monthly measurements

**Format:** Merged CSV (850 MB) joining agricultural statistics with climate data via county FIPS codes and year.

**Key attributes:**

*Categorical (4):*

- `COUNTY_NAME`: 254 Texas counties (plus 2 aggregated entries: "OTHER" and combined county data)
- `COMMODITY_DESC`: 165 crop types (cotton, wheat, corn, sorghum dominate)
- `STATISTICCAT_DESC`: 16 measurement types (yield, production, area harvested, price, etc.)
- `YEAR`: 2000-2023 (24 years)

*Climate features (72):*

- Monthly measurements (60 features): Precipitation, max/min/avg temperature, cooling/heating degree days for each month (Jan-Dec)
- Engineered aggregates (12 features): Growing season totals (Apr-Sep) and annual averages
  - Example ranges: Annual precipitation 3-95 inches, annual avg temperature 60-75°F

*Target variable:*

- `VALUE`: Continuous numeric ranging from 0.1 to 2.8 billion depending on statistic type and units (bushels, acres, dollars)

**What makes it suitable:** The merged dataset directly connects climate conditions to agricultural outcomes at the county-year level, allowing us to model how weather affects crops. Our EDA revealed clear regional patterns (e.g., correlation between growing season temperature and corn yield: r = -0.70).

## Methodology

**Baseline:** Decision Tree Regressor

- Naturally handles mixed categorical/numerical features without extensive preprocessing
- Captures non-linear relationships between climate and agriculture (e.g., optimal temperature ranges)
- Provides interpretable feature importance to identify which climate factors matter most
- Works well with our county-level spatial structure

**Improvement:** AdaBoost (Adaptive Boosting)

- Combines multiple weak decision trees to reduce bias and variance
- Expected 10-20% RMSE improvement over baseline based on similar agricultural studies
- More robust to outliers (e.g., extreme drought/flood years)
- Maintains interpretability through feature importance aggregation

**Optional exploration:** We may experiment with PCA for dimensionality reduction given our 120 features, but interpretability trade-offs will be carefully evaluated since stakeholders need to understand which climate factors drive predictions.

## Teaming Strategy

**Meeting schedule:** Weekly calls on Sunday afternoons + ongoing collaboration via Discord and GitHub

**Timeline:**

- **Week 1:** Data preprocessing complete (missing values, encoding, train/test split)
- **Week 1.5:** Implement and evaluate baseline Decision Tree model
- **Week 2:** AdaBoost implementation and hyperparameter tuning
- **Week 2.5:** Final evaluation, create visualizations, error analysis
- **Week 3:** Write report and prepare presentation

### Role Assignments and Commitment Matrix

| Task | Primary | Support | Deadline |
|------|---------|---------|----------|
| Data preprocessing & feature engineering | Carter | Johann | Nov 14 |
| Baseline Decision Tree implementation | Johann | Jay | Nov 21 |
| AdaBoost improvements | Jay | Carter | Nov 23 |
| Model evaluation & visualization | Carter | All | Nov 23 |
| Report writing | All | All | Nov 28 |
| Presentation preparation | Jay | All | Dec 3 |

**Collaboration tools:**

- GitHub for code version control
- Jupyter notebooks for reproducible analysis
- Discord for questions and coordination
- Weekly check-ins to review progress and adjust assignments

**Ensuring success:**

- Each member commits to working on their task each week
- Shared status updates with clear deadlines
- Early communication if someone falls behind on their tasks

## Mitigation Plan

**Data alternative:** If the climate merge has issues, we'll use temporal/spatial features (year, county, crop type) as baseline predictors. These alone capture trends and regional differences. We could also incorporate lagged agricultural variables (previous year's yield).

**Method alternative:**

- If Decision Tree performs poorly (R² < 0.50), we'll switch to Random Forest as the baseline
- If AdaBoost doesn't improve results by at least 5%, we'll try Gradient Boosting or XGBoost
- If tree-based models fail entirely, we'll fall back to regularized linear regression (Ridge/Lasso)

**Task alternative:** If any timeline component slips by more than 3 days, we'll adjust by simplifying the improvement phase (e.g., skip PCA experiments) while maintaining core baseline + AdaBoost requirements.

**Teammate MIA:**

- If someone is unresponsive for >48 hours, we redistribute their tasks among remaining members
- Critical path tasks (baseline implementation) get priority reassignment

**Baseline GIGO (Garbage In, Garbage Out):**

- Our EDA shows the data is 93% complete overall, with climate measurements at 96.5% completeness and no impossible values (e.g., negative yields)
- If we discover issues during modeling, we'll implement data validation checks:
  - Remove records with missing target values
  - Filter outliers beyond 3 standard deviations for continuous variables
  - Verify climate values are within physically possible ranges
- Worst case: Focus on a subset of high-quality data (major crops + complete climate records only)
