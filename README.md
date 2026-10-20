# MSP Predictor
### Agriculture Market Price Prediction and Trend Analysis System

**Status:** Under Development  
**Current completed phase:** Historical data integration, preprocessing, validation and cleaning.

## 1. Project Overview

MSP Predictor is a data-driven agricultural market intelligence project focused on historical mandi prices in Haryana, India. It aims to support statistical analysis, market-wise comparisons and, in later phases, machine-learning-based estimates of future mandi modal prices.

The project is intended to help farmers and traders make better-informed selling and purchasing decisions. **Despite its name, the project's prediction target is the future mandi modal price, not the government-declared Minimum Support Price (MSP).** Government MSP comparison may be explored as a future extension.

## 2. Project Objectives

- Collect and prepare historical agricultural mandi price records.
- Analyze historical minimum, maximum and modal prices.
- Explore seasonal, year-wise and month-wise price trends.
- Compare prices across Haryana mandis.
- Develop and evaluate models for future modal-price prediction.
- Present results through an interactive dashboard.
- **Optional future feature:** Estimate the potential net sale proceeds across mandis after accounting for travel distance and transportation costs.

## 3. Initial Coverage

**State:** Haryana  
**Historical period:** 2021–2025

Five crop categories are in scope:

1. Wheat
2. Paddy — Basmati and Common
3. Bajra (Pearl Millet/Cumbu)
4. Mustard
5. Cotton

Paddy has two separate dataset entries, so six commodity entries represent the five crop categories.

## 4. Dataset Information

The current historical datasets were obtained as five yearly CSV files from Kaggle: `2021.csv`, `2022.csv`, `2023.csv`, `2024.csv`, and `2025.csv`. The Data.gov.in mandi API was explored, but reliable access to sufficient historical records was a challenge; live API integration is not claimed as complete.

| Processing stage | Records |
| --- | ---: |
| Combined five-year dataset | 23,822,963 |
| Haryana-only records | 964,476 |
| Selected commodity records (before cleaning) | 38,832 |
| Invalid records removed | 11 |
| **Final cleaned records** | **38,821** |

**Final cleaned dataset:** 38,821 rows × 12 columns.

Original fields: `State`, `District`, `Market`, `Commodity`, `Variety`, `Grade`, `Arrival_Date`, `Min_Price`, `Max_Price`, `Modal_Price`, `Commodity_Code`.

Derived field: `Year`, extracted from `Arrival_Date`.

> Dataset files are not necessarily included in this repository. Their redistribution and licensing should be checked before uploading them.

## 5. Technology Stack

### Currently used

| Technology | Usage |
| --- | --- |
| Python | Data processing and validation |
| Pandas | CSV loading, integration, filtering and cleaning |
| Jupyter Notebook | Development and exploration |
| CSV | Historical data storage format |

### Planned

| Technology | Intended usage |
| --- | --- |
| NumPy | Numerical operations |
| Matplotlib / Seaborn | Statistical visualization |
| Scikit-learn | Regression model training and evaluation |
| Linear Regression | Initial ML baseline |
| MySQL | Relational data storage |
| Flask | Backend and prediction API |
| HTML, CSS, JavaScript | Interactive web dashboard |
| Plotly (optional) | Interactive charts |

## 6. Work Completed

### Historical data collection and integration

- Loaded five yearly CSV datasets using `pandas.read_csv()`.
- Combined them using `pandas.concat()` into 23,822,963 records.
- Filtered Haryana records (964,476 rows).
- Explored 118 unique Haryana commodity names and selected the target crop entries (38,832 rows).

### Preprocessing and data validation

- Converted `Arrival_Date` to datetime and extracted `Year`.
- Checked year-wise crop availability using `pandas.crosstab()`.
- Inspected data types, missing values and exact duplicates.
- Found **0 missing values** and **0 exact duplicate records** in the selected data.
- Applied logical price constraints:

```text
Min_Price > 0
Max_Price > 0
Modal_Price > 0
Min_Price <= Modal_Price <= Max_Price
```

- Identified and removed 11 unique invalid records (some violated multiple checks).
- Prepared **38,821 valid historical mandi records** for subsequent analysis.

**The preprocessing and cleaning phase is complete.** Statistical analysis, ML training, the web application and deployment are not yet complete.

## 7. Development Status

| Module | Status |
| --- | --- |
| Historical data collection | Completed |
| Dataset integration | Completed |
| Haryana and commodity filtering | Completed |
| Date preprocessing | Completed |
| Data-quality checks and price validation | Completed |
| Removal of invalid records | Completed |
| Exploratory data analysis (EDA) | Pending |
| Mean, median, variance and standard deviation | Pending |
| Year-wise/month-wise charts and mandi comparisons | Pending |
| Probability and trend analysis | Pending |
| Feature engineering | Pending |
| Historical-average baseline | Pending |
| Linear Regression training | Pending |
| MAE-based model evaluation | Pending |
| MySQL integration | Pending |
| Flask backend and prediction API | Pending |
| Interactive dashboard | Pending |
| End-to-end testing and deployment | Pending |
| Optional distance-based mandi recommendation | Future enhancement — not implemented |

## 8. Planned Machine Learning Approach

- **Learning type:** Supervised learning
- **Problem:** Regression
- **Target:** Future `Modal_Price` for a selected commodity and mandi
- **Initial model:** Linear Regression
- **Primary evaluation metric:** Mean Absolute Error (MAE)
- **Evaluation approach:** Chronological/time-aware train-test split to help avoid future-data leakage

Planned workflow:

```text
Cleaned historical dataset
       ↓
EDA and statistical analysis
       ↓
Feature engineering
       ↓
Chronological train/test split
       ↓
Historical-average baseline
       ↓
Linear Regression training
       ↓
Future modal-price prediction
       ↓
MAE evaluation and model improvement
```

**No model has been trained or evaluated yet.** No prediction accuracy, MAE score or confidence estimate is currently available.

## 9. Planned Dashboard Features

- Select crop, district and mandi (where data is available).
- View available historical minimum, maximum and modal prices.
- Display mean, median, variance and standard deviation.
- Explore time-series charts and compare Haryana mandis.
- Show model-supported future modal-price estimates.
- Display next-week trends and next-month average estimates when sufficient data and validated models are available.
- Indicate predicted Increase, Decrease or Stable movement; show probability/confidence only if the implemented method can justify it.

These are **planned features**, not currently deployed functionality.

## 10. Optional Future Feature: Distance-Based Mandi Profit Estimator

**Status: Proposed optional enhancement — not implemented.**

A future decision-support module could help a farmer compare potential returns from selling the same crop at different mandis. For a user-selected crop, quantity and starting location, it would:

1. Obtain predicted modal prices for multiple eligible mandis for a chosen future date (for example, two days ahead), **only after** reliable market-wise forecasting has been implemented and evaluated.
2. Estimate road distance from the farmer's starting location to each mandi using a suitable routing/distance data source.
3. Estimate transportation cost using distance, vehicle cost per kilometre and any relevant fixed charges.
4. Compare estimated sale proceeds after transport costs, and optionally rank mandis by estimated net proceeds.

### Illustrative calculation

If price is expressed in **₹/quintal**, quantity in **quintals**, distance in **km**, and transport rate in **₹/km**:

```text
Estimated gross sale value = Predicted mandi price (₹/quintal) × Quantity (quintals)
Estimated transport cost   = Road distance (km) × Transport rate (₹/km) + Other charges
Estimated net sale proceeds = Estimated gross sale value − Estimated transport cost

Estimated advantage over a reference mandi =
    Estimated net proceeds at candidate mandi −
    Estimated net proceeds at reference mandi
```

**Example (hypothetical only):** For 20 quintals of wheat, a predicted price of ₹2,500/quintal gives ₹50,000 gross sale value. If estimated transport and other charges are ₹1,500, estimated net sale proceeds are ₹48,500. These are not guaranteed earnings or actual farming profit: crop production costs, commissions, loading/unloading, quality deductions, and other expenses may also apply.

A simple **distance-weighted average price** could be shown as an additional comparison statistic if useful, but the recommendation should primarily compare **net proceeds for each mandi**, rather than assuming that an average price represents a realizable selling price.

**Dependencies and limitations:** This feature would require validated market-specific forecasts, user-supplied quantity and origin, a road-distance source, and transport-cost assumptions. Forecast errors, actual bids, crop quality and travel costs may change the outcome. No live routing integration or mandi-profit calculation has been built yet.

## 11. Future Scope

- Expand beyond Haryana and add more crops.
- Integrate weather, rainfall, production and supply-demand indicators if reliable datasets become available.
- Explore import/export and broader economic factors.
- Compare advanced ML models after establishing meaningful baselines.
- Add official MSP versus mandi-price comparison and possibly separate MSP forecasting research.
- Explore the **optional distance-based mandi net-proceeds comparison** described above.

## 12. Documentation and Project Details

The project follows the **Software Requirements Specification (SRS v1.0)**, which documents its initial scope, architecture, functional/non-functional requirements, build stages and acceptance criteria. This README also records the current practical implementation status and the optional enhancement proposal.

- **Project:** MSP Predictor — Agriculture Market Price Prediction and Trend Analysis System
- **Domain:** Agriculture, Data Analytics, Statistics and Machine Learning
- **Institution:** GLA University, Greater Noida
- **Program:** B.Tech, Computer Science and Engineering – AI
- **Developer:** Peeyush Panwar
- **Status:** Under Development

## 13. Development Roadmap

- [x] Collect historical datasets (2021–2025)
- [x] Integrate yearly datasets
- [x] Filter Haryana and target commodities
- [x] Parse dates and derive year
- [x] Validate and clean price records
- [ ] Perform EDA and statistical analysis
- [ ] Create historical charts and mandi comparisons
- [ ] Engineer features and establish historical-average baseline
- [ ] Train Linear Regression and evaluate MAE
- [ ] Integrate MySQL and Flask
- [ ] Build the interactive dashboard
- [ ] Test and deploy the complete system
- [ ] **Optional:** Implement distance-based mandi net-proceeds comparison

## 14. Disclaimer

This is an academic project. Future predicted mandi prices will be estimates, not guaranteed prices or official government MSP announcements. Any future mandi comparison or transport-cost recommendation will be indicative and should not be treated as a guaranteed profit calculation.
