# Netflix Data Science & Analytics Project

## Project Overview

An end-to-end Data Science and Data Analytics project built around a Netflix content dataset.

The project covers the complete analytical workflow, starting from raw data cleaning and exploratory analysis, then progressing through recommendation systems, time-series forecasting, machine learning classification, business analysis, and an interactive Power BI dashboard.

The goal of the project is not only to build models, but also to translate data into meaningful insights that can support business understanding and decision-making.

---

## Project Highlights

- Cleaned and validated a Netflix catalog dataset containing approximately **8,787 unique titles**
- Performed exploratory analysis across content type, country, rating, release year, duration, and genre
- Built a **content-based recommendation system** using TF-IDF and cosine similarity
- Developed and evaluated multiple **time-series forecasting models**
- Built and compared multiple **machine learning classification models**
- Applied feature selection and target-leakage prevention
- Tuned a Random Forest classifier using cross-validation and RandomizedSearchCV
- Defined business questions and translated them into analytical metrics and visualizations
- Built an interactive **Power BI dashboard** to communicate the main business insights

---

# Power BI Dashboard

The project includes a Power BI dashboard designed to summarize the main Netflix catalog insights in an interactive and business-friendly format.

### Dashboard Features

- Total Titles
- Total Movies
- Total TV Shows
- Total Countries
- Movies vs TV Shows distribution
- Top 10 Countries by catalog size
- Netflix catalog additions over time
- Content rating distribution
- Top 10 content genres
- Interactive filters for content analysis

![Netflix Power BI Dashboard](assets/netflix_dashboard.png)

### Power BI File

[Download the Power BI Dashboard](Netflix%20Portfolio.pbix)

> The `.pbix` file can be opened using Power BI Desktop.

---

# Business Objective

The business analysis focuses on understanding the composition and evolution of the Netflix content catalog and identifying meaningful patterns across content type, geographic representation, ratings, genres, and catalog growth over time.

The analysis addresses the following business questions:

1. How is the Netflix catalog distributed between Movies and TV Shows?
2. Which countries contribute the most titles to the Netflix catalog?
3. How has the number of titles added to Netflix changed over time?
4. What are the most common content ratings in the Netflix catalog?
5. What are the most common genres in the Netflix catalog?

---

# Key Business Insights

- Movies represent approximately **70%** of the titles in the analyzed catalog, while TV Shows account for about **30%**.
- The **United States** has the highest number of titles in the dataset, followed by India and the United Kingdom.
- Catalog additions increased significantly during the late 2010s and reached their highest level in **2019**.
- The lower number of additions in 2021 should be interpreted carefully because the dataset contains only a partial year.
- **TV-MA** is the most common content rating, followed by TV-14.
- **International Movies, Dramas, and Comedies** are among the most frequently represented content categories.
- Genre analysis is multi-label, meaning a single title can contribute to multiple genre categories.

---

# Project Tasks

## Task 1 — Data Cleaning & Preprocessing

The raw Netflix dataset was inspected and prepared for further analysis.

Key steps included:

- Dataset structure and data-type inspection
- Missing-value analysis
- Duplicate detection
- Replacement of unavailable country and director values with `Unknown`
- Conversion of date fields
- Removal of duplicated content records
- Final dataset validation
- Export of the cleaned dataset as `netflix_cleaned.csv`

---

## Task 2 — Exploratory Data Analysis

Exploratory analysis was performed to understand the structure and distribution of Netflix content.

Analysis included:

- Movies vs TV Shows
- Top contributing countries
- Rating distribution
- Netflix content categories
- Titles added over time
- Release-year trends
- Movie duration analysis
- TV Show season analysis

Visualizations were created using Python and interpreted to identify meaningful content patterns.

---

## Task 3 — Content-Based Recommendation System

A content-based recommendation system was developed using Netflix metadata.

### Features Used

- `listed_in`
- `director`
- `type`
- `rating`

### Methodology

- Text preprocessing
- Feature combination
- TF-IDF Vectorization
- Cosine Similarity
- Reusable recommendation function
- Case-insensitive title search
- Missing-title handling

### Recommendation Evaluation

Recommendation quality was evaluated using:

- Category Overlap
- Same Content Type Rate

These metrics evaluate metadata similarity rather than classification accuracy.

Because the dataset does not contain user viewing history, ratings, or watch behavior, the system is an exploratory content-based recommendation engine rather than a personalized recommender.

---

## Task 4 — Time Series Forecasting

Historical Netflix content trends were analyzed and forecasting models were developed.

### Modeling Period

The forecasting analysis focused on:

**2000–2020**

The year 2021 was excluded from training because the dataset only contains records through September 2021.

### Train/Test Split

- Training: 2000–2015
- Testing: 2016–2020

Chronological splitting was used to preserve the time-series structure.

### Models Evaluated

| Model | MAE |
| --- | ---: |
| Baseline | 456.40 |
| Holt Linear Trend | 212.74 |
| Damped Holt | **210.37** |

The Damped Holt model achieved the lowest Mean Absolute Error.

### Forecasting Limitation

The forecasting target was based on `release_year`, which represents the original release year of a title rather than necessarily the date it was added to Netflix.

Therefore, the forecast should be interpreted as trend extrapolation rather than a direct prediction of future Netflix catalog additions.

---

## Task 5 — Machine Learning Classification

A classification system was developed to predict whether a Netflix title is a:

- Movie
- TV Show

### Target Distribution

- Movies: approximately **69.7%**
- TV Shows: approximately **30.3%**

Because of this moderate class imbalance, model evaluation included class-specific metrics in addition to accuracy.

### Final Features

- `country`
- `release_year`
- `rating`
- `added_year`

### Leakage Prevention

Several columns were excluded because they could introduce leakage or poor generalization:

- `show_id`
- `title`
- `director`
- `duration`
- `listed_in`
- original `date_added`

For example, `duration` directly reveals the target because Movies are measured in minutes while TV Shows are measured in seasons.

### Data Preparation

- 80% training data
- 20% testing data
- Stratified train/test split
- One-Hot Encoding for categorical variables
- StandardScaler for numerical variables
- Scikit-learn pipelines
- ColumnTransformer

---

## Model Comparison

| Model | Accuracy | TV Show Recall | TV Show F1 | Macro F1 |
| --- | ---: | ---: | ---: | ---: |
| Baseline | 69.68% | 0.00 | 0.00 | 0.41 |
| Logistic Regression | 77.47% | 0.44 | 0.54 | 0.70 |
| Decision Tree | 77.19% | 0.55 | 0.59 | 0.72 |
| Regularized Decision Tree | 76.17% | 0.30 | 0.44 | 0.64 |
| Random Forest | 78.38% | 0.58 | 0.62 | 0.73 |
| Tuned Random Forest | **78.61%** | 0.57 | **0.62** | **0.73** |

---

## Final Classification Model

The **Tuned Random Forest** was selected as the final model.

Hyperparameter tuning was performed using:

- RandomizedSearchCV
- 30 parameter combinations
- 5-fold Cross-Validation
- Macro F1 optimization

Best Cross-Validation Macro F1:

**0.733**

Best parameters:

- `n_estimators = 200`
- `min_samples_split = 5`
- `min_samples_leaf = 1`
- `max_features = log2`
- `max_depth = None`

The tuned model achieved:

- Test Accuracy: **78.61%**
- Movie F1-score: **0.85**
- TV Show F1-score: **0.62**
- Macro F1-score: **0.73**

---

## Task 6 — Business Insights & Dashboard

The final task translated technical analysis into business-oriented insights.

The workflow included:

- Business understanding
- Business objective definition
- Business question development
- Metric selection
- Visualization selection
- Insight interpretation
- Data limitation analysis
- Power BI dashboard development

The dashboard provides an interactive summary of Netflix catalog composition, geographic distribution, content ratings, genres, and growth trends.

---

# Technologies Used

### Programming & Data Analysis

- Python
- Pandas
- NumPy

### Data Visualization

- Matplotlib
- Seaborn
- Power BI

### Machine Learning

- Scikit-learn
- Logistic Regression
- Decision Tree
- Random Forest
- Dummy Classifier
- RandomizedSearchCV
- Cross-Validation
- Pipelines
- ColumnTransformer
- One-Hot Encoding
- StandardScaler

### Recommendation Systems

- TF-IDF Vectorization
- Cosine Similarity

### Time Series

- Statsmodels
- Holt Linear Trend
- Damped Holt Trend
- Mean Absolute Error

### Development Tools

- Google Colab
- Git
- GitHub
- Power BI Desktop

---

# Project Workflow

```text
Raw Data
   ↓
Data Cleaning & Validation
   ↓
Exploratory Data Analysis
   ↓
Recommendation System
   ↓
Time Series Analysis
   ↓
Forecasting Model Development
   ↓
Machine Learning Feature Selection
   ↓
Leakage Prevention
   ↓
Classification Modeling
   ↓
Model Evaluation
   ↓
Hyperparameter Tuning
   ↓
Business Understanding
   ↓
Business Questions & Metrics
   ↓
Business Insights
   ↓
Power BI Dashboard
