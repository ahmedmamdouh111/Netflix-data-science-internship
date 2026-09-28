# Netflix Data Science Internship Project

## Project Overview

A practical end-to-end Data Science internship project based on a Netflix dataset.

The project covers data cleaning, exploratory data analysis, content-based recommendation systems, time series analysis, forecasting, machine learning classification, model evaluation, and business insights.

The project is being developed incrementally throughout the internship. Tasks 1–5 are currently completed, while Task 6 will be added in the next stage.

---

## Current Progress

- [x] Task 1 – Data Cleaning & Preprocessing
- [x] Task 2 – Exploratory Data Analysis (EDA)
- [x] Task 3 – Recommendation System Analysis
- [x] Task 4 – Trend Prediction Analysis
- [x] Task 5 – Machine Learning Classification Model
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

### Task 5 – Machine Learning Classification Model

Developed and evaluated machine learning classification models to predict whether Netflix content is a **Movie** or **TV Show**.

#### Target Definition

The classification target was:

- `type` – Movie or TV Show

The dataset showed a moderate class imbalance:

- Movies: approximately **69.7%**
- TV Shows: approximately **30.3%**

Because of this imbalance, model evaluation was not based on accuracy alone.

---

#### Feature Selection

Each dataset column was reviewed before modeling to identify useful predictors and prevent target leakage.

Final modeling features:

- `country`
- `release_year`
- `rating`
- `added_year`

Several features were excluded:

- `show_id` – unique identifier with no meaningful predictive value.
- `title` – almost every title is unique, making it a high-cardinality identifier-like feature.
- `director` – very high cardinality with many rare values and a large number of `Unknown` entries.
- `duration` – directly reveals the target because Movies are measured in minutes while TV Shows are measured in seasons.
- `listed_in` – contains categories such as `Movies` and `TV Shows`, introducing direct target leakage.
- `date_added` – transformed into the simpler numerical feature `added_year`.

An additional engineered feature, `years_to_netflix`, was initially explored as:

`added_year - release_year`

However, validation revealed several negative values, showing that the two fields were not consistently comparable across all titles, particularly TV Shows.

The feature was therefore excluded from modeling.

---

#### Train/Test Split

The dataset was divided into:

- **80% Training Data**
- **20% Testing Data**

A stratified split was used to preserve the Movie/TV Show distribution in both sets.

The final split contained:

- Training samples: **7,029**
- Testing samples: **1,758**

---

#### Data Preprocessing

A Scikit-learn `ColumnTransformer` was used to apply different preprocessing steps to categorical and numerical features.

Categorical features:

- `country`
- `rating`

Preprocessing:

- **One-Hot Encoding**
- `handle_unknown='ignore'` to safely handle unseen categories

Numerical features:

- `release_year`
- `added_year`

Preprocessing:

- **StandardScaler**

The preprocessing steps were integrated into machine learning pipelines so that transformations were fitted only on the training data, helping prevent data leakage.

---

#### Baseline Model

A `DummyClassifier` using the most frequent class was created as a baseline.

Because Movies represent approximately 70% of the dataset, the baseline simply predicted the majority class.

Baseline Accuracy:

**69.68%**

This provided a reference point for determining whether the machine learning models were learning meaningful patterns.

---

#### Classification Models

The following models were evaluated:

- Dummy Classifier
- Logistic Regression
- Decision Tree
- Regularized Decision Tree
- Random Forest
- Tuned Random Forest

Model performance was evaluated using:

- Accuracy
- Precision
- Recall
- F1-score
- Macro F1-score
- Confusion Matrix

---

#### Logistic Regression

Logistic Regression achieved:

- Accuracy: **77.47%**
- Movie Recall: **0.92**
- TV Show Recall: **0.44**
- TV Show F1-score: **0.54**
- Macro F1-score: **0.70**

The model significantly outperformed the baseline.

However, the confusion matrix showed that Logistic Regression performed much better on Movies than TV Shows.

It correctly identified most Movies but detected only about 44% of the TV Shows.

---

#### Decision Tree

The Decision Tree achieved:

- Test Accuracy: **77.19%**
- TV Show Recall: **0.55**
- TV Show F1-score: **0.59**
- Macro F1-score: **0.72**

The model improved TV Show detection compared with Logistic Regression.

However, model complexity inspection showed:

- Tree depth: **50**
- Number of leaves: **1,058**
- Training Accuracy: **87.42%**
- Test Accuracy: **77.19%**

The large train-test performance gap indicated noticeable overfitting.

---

#### Regularized Decision Tree

A more constrained Decision Tree was tested using parameters such as:

- `max_depth`
- `min_samples_split`
- `min_samples_leaf`

The regularized model achieved:

- Training Accuracy: **76.91%**
- Test Accuracy: **76.17%**
- TV Show Recall: **0.30**
- TV Show F1-score: **0.44**
- Macro F1-score: **0.64**

Regularization significantly reduced the train-test gap but also reduced predictive performance, suggesting that the selected constraints introduced additional bias.

---

#### Random Forest

Random Forest was introduced as an ensemble model using multiple Decision Trees.

The original Random Forest achieved:

- Training Accuracy: **87.42%**
- Test Accuracy: **78.38%**
- Movie Recall: **0.87**
- TV Show Recall: **0.58**
- TV Show F1-score: **0.62**
- Macro F1-score: **0.73**

Random Forest achieved stronger overall performance and improved TV Show detection compared with the previous models.

However, the train-test accuracy gap of approximately **9.04 percentage points** indicated some remaining overfitting.

---

#### Hyperparameter Tuning

Random Forest hyperparameters were optimized using **RandomizedSearchCV**.

The search used:

- **30 parameter combinations**
- **5-fold Cross-Validation**
- **Macro F1-score** as the optimization metric

Macro F1 was selected because it gives equal importance to Movies and TV Shows despite the class imbalance.

The best parameter configuration was:

- `n_estimators = 200`
- `min_samples_split = 5`
- `min_samples_leaf = 1`
- `max_features = log2`
- `max_depth = None`

Best Cross-Validation Macro F1:

**0.733**

---

#### Tuned Random Forest

The final tuned Random Forest achieved:

- Training Accuracy: **86.33%**
- Test Accuracy: **78.61%**
- Movie Precision: **0.82**
- Movie Recall: **0.88**
- Movie F1-score: **0.85**
- TV Show Precision: **0.67**
- TV Show Recall: **0.57**
- TV Show F1-score: **0.62**
- Macro F1-score: **0.73**

The train-test accuracy gap decreased from approximately:

**9.04 percentage points → 7.72 percentage points**

indicating a modest improvement in generalization.

---

#### Final Model Comparison

| Model | Accuracy | TV Show Recall | TV Show F1 | Macro F1 |
| --- | ---: | ---: | ---: | ---: |
| Baseline | 69.68% | 0.00 | 0.00 | 0.41 |
| Logistic Regression | 77.47% | 0.44 | 0.54 | 0.70 |
| Decision Tree | 77.19% | 0.55 | 0.59 | 0.72 |
| Regularized Decision Tree | 76.17% | 0.30 | 0.44 | 0.64 |
| Random Forest | 78.38% | 0.58 | 0.62 | 0.73 |
| Tuned Random Forest | **78.61%** | 0.57 | 0.62 | **0.73** |

---

#### Final Model Selection

The **Tuned Random Forest** was selected as the final classification model.

Although the performance difference between the original and tuned Random Forest was relatively small, the tuned model achieved:

- The highest test accuracy.
- The highest overall Macro F1 among the evaluated configurations.
- Strong class-level performance.
- A smaller train-test performance gap than the original Random Forest.

The results also demonstrate why accuracy alone should not be used to evaluate imbalanced classification problems.

Class-specific Recall, F1-score, Macro F1, and confusion matrices provided a more complete understanding of model performance.

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

### Machine Learning & Classification

- Scikit-learn
- Logistic Regression
- Decision Tree
- Random Forest
- Dummy Classifier
- Pipeline
- ColumnTransformer
- One-Hot Encoding
- StandardScaler
- RandomizedSearchCV
- Cross-Validation
- Classification Metrics
- Confusion Matrix

### Development Tools

- Google Colab
- Git
- GitHub

---

## Repository Files

| File | Description |
| --- | --- |
| [Netflix_Data_Science_Internship.ipynb](Netflix_Data_Science_Internship.ipynb) | Main project notebook containing Tasks 1–5. |
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

**Forecasting Model Evaluation**

↓

**Machine Learning Feature Selection**

↓

**Leakage Prevention & Data Preparation**

↓

**Classification Modeling**

↓

**Model Evaluation**

↓

**Cross-Validation & Hyperparameter Tuning**

↓

**Final Model Selection**

Task 6 will extend the project with business-focused insights and dashboard development.

---

## Project Status

**Work in Progress**

Tasks 1–5 are completed.

Upcoming work:

- Task 6 – Data Science Business Insights Dashboard

Further portfolio enhancements may later include an interactive dashboard and model deployment.

---

## Author

**Ahmed Mamdouh**

[GitHub](https://github.com/ahmedmamdouh111) · [LinkedIn](https://linkedin.com/in/ahmed-mamdouh831)
