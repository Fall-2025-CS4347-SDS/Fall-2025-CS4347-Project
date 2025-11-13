# Texas Agriculture Climate ML Project

Machine learning project predicting agricultural statistics for Texas counties using climate data.

## Dataset

- **Size:** 398,204 records × 120 features = 47,784,480 data points
- **Sources:** USDA NASS QuickStats + NOAA nClimDiv climate data
- **Coverage:** 255 Texas counties, 2000-2023, 165 crop types

## Project Structure

```
.
├── data_preparation.ipynb           # Complete data pipeline
├── requirements.txt                 # Python dependencies
├── PROJECT_PROPOSAL_CONCISE.md      # Formal project proposal
├── TEAM_SUMMARY.md                  # Team-facing documentation
└── source-data/                     # Raw data (not in repo - see below)
```

## Setup

1. **Clone the repository:**
   ```bash
   git clone <repo-url>
   cd Project
   ```

2. **Create virtual environment:**
   ```bash
   python -m venv .venv
   .venv\Scripts\activate  # Windows
   # or
   source .venv/bin/activate  # Mac/Linux
   ```

3. **Install dependencies:**
   ```bash
   pip install -r requirements.txt
   ```

4. **Download data:**
   - Raw data files are NOT included in the repository (too large)
   - Download USDA NASS QuickStats data: [QuickStats](https://quickstats.nass.usda.gov/)
   - Download NOAA nClimDiv data: [nClimDiv](https://www.ncei.noaa.gov/pub/data/cirs/climdiv/)
   - Place files in `source-data/` directory

## Usage

Open and run `data_preparation.ipynb` to:
1. Load raw USDA NASS data (23.3M records nationwide)
2. Filter to Texas county-level data (398K records)
3. Parse NOAA climate files (fixed-width format)
4. Merge agricultural + climate data (96.5% success rate)
5. Export final dataset: `texas_agriculture_with_climate_2000_2023.csv`

## Methods

- **Baseline:** Decision Tree Regressor
- **Improvement:** AdaBoost Ensemble
- **Dimensionality:** PCA analysis
- **Evaluation:** Temporal train/test split (2000-2018 / 2020-2023)

## Team

- Carter Dobbs
- Johann Steinhoff
- Jay Suh

## License

Academic project for CS 4347 - Intro to Machine Learning
