# Netflix Data Science Internship Project

## Project Overview

A practical end-to-end Data Science internship project based on a Netflix dataset.

The project covers data cleaning, exploratory data analysis, content-based recommendation systems, time series analysis, forecasting, machine learning, and business insights.

The project is being developed incrementally throughout the internship. Tasks 1–4 are currently completed, while Tasks 5–6 will be added as the internship progresses.

---

## Current Progress

- [x] Task 1 – Data Cleaning & Preprocessing
- [x] Task 2 – Exploratory Data Analysis (EDA)
- [x] Task 3 – Recommendation System Analysis
- [x] Task 4 – Trend Prediction Analysis
- [ ] Task 5 – Machine Learning Classification Model
- [ ] Task 6 – Data Science Business Insights Dashboard

---

## Completed Work

### Task 1 – Data Cleaning & Preprocessing

Prepared the raw Netflix dataset for further analysis and modeling.

Key steps:

- Inspected dataset structure, data types, and column values.
- Checked missing values and duplicate records.
- Replaced unavailable `country` and `director` values labeled `Not Given` with `Unknown`.
- Converted `date_added` to datetime format.
- Detected duplicate content while excluding the unique `show_id`.
- Removed duplicated content records while keeping the first occurrence.
- Validated missing values, duplicate records, and data types after cleaning.
- Saved the cleaned dataset as `netflix_cleaned.csv`.

---

### Task 2 – Exploratory Data Analysis

Explored the Netflix dataset to identify patterns, distributions, and content trends.

Analysis included:

- Movies vs TV Shows distribution
- Top contributing countries
- Most common Netflix categories
- Release-year content trends
- Rating distribution
- Titles added to Netflix over time
- Movie duration analysis
- TV Show season analysis

Visualizations were created using Matplotlib and Seaborn, followed by interpretation of the main findings.

---

### Task 3 – Recommendation System Analysis

Developed an exploratory content-based recommendation system using Netflix metadata.

Features used included:

- `listed_in`
- `director`
- `type`
- `rating`

Key steps:

- Preprocessed text-based content features.
- Removed spaces from director names and converted them to lowercase so full names behave as meaningful tokens.
- Combined relevant metadata into a content feature representation.
- Applied **TF-IDF Vectorization** for feature extraction.
- Calculated content similarity using **Cosine Similarity**.
- Developed a reusable `recommendation_system()` function.
- Added case-insensitive title matching.
- Added configurable recommendation counts.
- Handled titles that do not exist in the dataset.
- Tested the recommendation function using an edge case.

#### Recommendation Evaluation

Recommendation quality was evaluated using metadata-based metrics:

- **Category Overlap** – measures the intersection of categories between the selected title and its recommendations relative to their combined categories.
- **Same Content Type Rate** – measures the percentage of recommendations that share the same content type as the selected title.

These metrics evaluate metadata similarity and **do not represent classification accuracy**.

The dataset does not contain user interaction history, ratings, or watch behavior, so the system remains an exploratory content-based recommendation model rather than a personalized recommendation engine.

---

### Task 4 – Trend Prediction Analysis

Analyzed historical Netflix content trends and developed forecasting models to estimate future content patterns.

#### Data Preparation

- Aggregated the dataset by `release_year`.
- Calculated the number of titles represented in each year.
- Inspected release-year coverage and identified historical content patterns.
- Analyzed Movies and TV Shows separately to understand how each content type contributed to the overall trend.

The analysis showed that Movies accounted for most titles during the selected period, while TV Shows continued growing through 2020 even as the number of Movies declined after approximately 2018.

#### Modeling Period

The forecasting analysis focused on the period:

**2000–2020**

Earlier years contained relatively sparse and irregular observations, while the period from 2000 onward provided a continuous annual series with a clearer modern trend.

The year **2021 was excluded from model training** because the dataset only contains records added up to **September 25, 2021**, making it a partial year.

#### Time Series Train/Test Split

Because time series observations depend on chronological order, random shuffling was avoided.

The data was divided into:

- **Training period:** 2000–2015
- **Testing period:** 2016–2020

This simulates a realistic forecasting scenario where future observations are predicted using only past information.

#### Baseline Forecast

A simple naive baseline was created by assuming that all future values would remain equal to the last observed training value.

This baseline provided a reference point for evaluating whether more advanced forecasting models added meaningful predictive value.

#### Forecasting Models

Two trend-based forecasting approaches were evaluated:

- **Holt's Linear Trend Model**
- **Damped Holt Trend Model**

Holt's Linear Trend captures level and trend in the historical series.

The Damped Holt model reduces the strength of the projected trend over longer forecasting horizons, preventing the trend from increasing or decreasing indefinitely at the same rate.

#### Model Evaluation

Models were evaluated using **Mean Absolute Error (MAE)**.

Approximate results:

| Model | MAE |
| --- | ---: |
| Baseline | 456.40 |
| Holt Linear Trend | 212.74 |
| Damped Holt | 210.37 |

The Damped Holt model achieved the lowest MAE, providing a small improvement over the standard Holt model and a substantial improvement over the naive baseline.

#### Final Forecast

After model evaluation, the selected Damped Holt model was retrained using the complete modeling period from **2000–2020**.

The model generated forecasts for 2021–2025.

The resulting forecast indicated a gradual decline in the number of titles represented by release year over the forecast horizon.

Because a damped trend was used, the magnitude of the projected decline gradually decreases over time.

#### Forecast Limitations

The forecasting results should be interpreted carefully because:

- The dataset contains a relatively small number of annual observations.
- `release_year` represents the original release year of a title, not necessarily the year it was added to Netflix.
- The dataset only contains records added through September 25, 2021.
- Holt-based models primarily model level and trend.
- The models cannot account for unexpected external events or changes in Netflix's future content strategy.
- Future forecasts represent trend-based estimates rather than exact predictions of Netflix content volume.

---

## Technologies Used

### Programming & Data Analysis
- Python
- Pandas
- NumPy

### Data Visualization
- Matplotlib
- Seaborn

### Recommendation Systems
- Scikit-learn
- TF-IDF Vectorization
- Cosine Similarity

### Time Series & Forecasting
- Statsmodels
- Holt's Linear Trend
- Damped Trend Forecasting
- Mean Absolute Error (MAE)

### Development Tools
- Google Colab
- Git
- GitHub

---

## Repository Files

| File | Description |
| --- | --- |
| [Netflix_Data_Science_Internship.ipynb](Netflix_Data_Science_Internship.ipynb) | Main project notebook containing Tasks 1–4. |
| [Dataset.csv](Dataset.csv) | Original Netflix dataset. |
| [netflix_cleaned.csv](netflix_cleaned.csv) | Cleaned dataset generated during Task 1. |
| [README.md](README.md) | Project documentation and progress summary. |

---

## Run the Project

[![Open in Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/ahmedmamdouh111/Netflix-data-science-internship/blob/main/Netflix_Data_Science_Internship.ipynb)

1. Open the notebook in Google Colab using the badge above.
2. Connect to a Python runtime.
3. Select **Runtime → Run all** to execute the notebook in order.

The notebook loads the raw `Dataset.csv` directly from its GitHub raw file URL, so no manual repository cloning or dataset upload is required.

Task 1 generates `netflix_cleaned.csv`, which is then used by subsequent analysis tasks.

---

## Project Workflow

The project currently follows this Data Science workflow:

**Raw Data**

↓

**Data Cleaning & Preprocessing**

↓

**Exploratory Data Analysis**

↓

**Content-Based Recommendation System**

↓

**Time Series Trend Analysis**

↓

**Forecasting Model Development**

↓

**Model Evaluation**

↓

**Future Trend Forecasting**

Additional Machine Learning and Business Intelligence tasks will be added in the next stages of the internship.

---

## Project Status

**Work in Progress**

Tasks 1–4 are completed.

Upcoming work:

- Task 5 – Machine Learning Classification Model
- Task 6 – Data Science Business Insights Dashboard

The repository will continue to be updated as the internship progresses.

---

## Author

**Ahmed Mamdouh**

[GitHub](https://github.com/ahmedmamdouh111) · [LinkedIn](https://linkedin.com/in/ahmed-mamdouh831)
