# Predicting Texas Agricultural Production Using Climate Data

**CS 4347 - Introduction to Machine Learning | Fall 2025**

Machine learning project analyzing how climate affects agricultural outcomes in Texas counties.

## Key Findings

- **Climate explains 82%** of model feature importance after proper normalization
- **Temperature > Precipitation:** Temperature features are 2.7x more important than precipitation
- **Crop-specific models work better:** Corn R² = 0.667 vs mixed model R² = 0.074
- **Drought years = better predictions:** Model R² = 0.147 in drought years vs 0.052 in normal years
- **YIELD responds best to climate:** R² = 0.146 compared to PRODUCTION (0.097) or AREA metrics (0.071-0.081)

## Dataset

| Metric | Value |
|--------|-------|
| Total Records | 398,204 |
| Features | 120 (16 climate + spatial/temporal) |
| Coverage | 254 Texas counties, 2000-2023 |
| Crop Types | 165 |
| Sources | USDA NASS QuickStats + NOAA nClimDiv |

## Project Structure

```
├── final_report.md                  # Final project report
├── final_analysis.ipynb             # Main analysis notebook
├── data_preparation.ipynb           # Data merging pipeline
├── texas_agriculture_eda.ipynb      # Exploratory data analysis
├── progress_report.ipynb            # Mid-project progress report
├── PROJECT_PROPOSAL.md              # Original project proposal
├── figures/                         # Generated visualizations
│   ├── feature_importance_breakdown.png
│   ├── climate_feature_importance.png
│   ├── monthly_importance.png
│   ├── crop_climate_sensitivity.png
│   ├── corn_yield_drought_years.png
│   ├── measurement_type_comparison.png
│   └── final_model_comparison.png
├── source-data/                     # Raw data files (not in repo)
├── texas_agriculture_with_climate_2000_2023.csv  # Merged dataset
└── requirements.txt                 # Python dependencies
```

## Models & Results

| Model | Train R² | Test R² | Notes |
|-------|----------|---------|-------|
| Decision Tree | 0.097 | 0.072 | Baseline |
| **Random Forest** | 0.095 | **0.074** | Best overall |
| Gradient Boosting | 0.081 | 0.069 | Least overfitting |

### By Measurement Type (Random Forest)

| Type | R² | Records |
|------|-----|---------|
| YIELD | 0.146 | 20,016 |
| PRODUCTION | 0.097 | 43,364 |
| AREA PLANTED | 0.081 | 19,973 |
| AREA HARVESTED | 0.071 | 134,416 |

### Crop-Specific Climate Sensitivity

| Crop | R² (Climate-Only) | Top Feature |
|------|-------------------|-------------|
| Corn | 0.667 | ANNUAL_TEMP_AVG |
| Wheat | 0.375 | PRECIP_MAY |
| Cotton | 0.295 | TAVG_AUG |
| Sorghum | 0.251 | ANNUAL_PRECIP |

## Setup

```bash
# Clone repository
git clone https://github.com/Cooldogyum/Fall-2025-CS4347-Project.git
cd Fall-2025-CS4347-Project

# Create virtual environment
python -m venv .venv
.venv\Scripts\activate  # Windows
# source .venv/bin/activate  # Mac/Linux

# Install dependencies
pip install -r requirements.txt
```

## Data Sources

Raw data files are not included in the repository due to size. To reproduce:

1. **USDA NASS QuickStats:** <https://quickstats.nass.usda.gov/>
   - Filter: Texas, County level, 2000-2023

2. **NOAA nClimDiv:** <https://www.ncei.noaa.gov/pub/data/cirs/climdiv/>
   - Download climate division files for Texas

Place downloaded files in `source-data/` and run `data_preparation.ipynb`.

## Team

| Name | NetID | Role |
|------|-------|------|
| Carter Dobbs | dgi6 | Project lead, data pipeline, analysis |
| Johann Steinhoff | ngq7 | Baseline modeling, hyperparameter tuning |
| Jay Suh | hkm55 | Ensemble methods, presentation |

## References

1. USDA NASS QuickStats: <https://quickstats.nass.usda.gov/>
2. NOAA nClimDiv: <https://www.ncei.noaa.gov/access/monitoring/climate-at-a-glance/>
3. scikit-learn: <https://scikit-learn.org/>

## License

Academic project for CS 4347 - Introduction to Machine Learning, TXST, Fall 2025
