# Netflix Data Science Internship Project

## Project Overview

A practical Data Science internship project based on a Netflix dataset. The planned scope covers data cleaning, exploratory data analysis, recommendation systems, machine learning, forecasting, and business insights.

The project is being developed incrementally throughout the internship. This **Version 1** README documents completed Tasks 1–3; Tasks 4–6 are planned for later.

## Current Progress

- [x] Task 1 – Data Cleaning & Preprocessing
- [x] Task 2 – Exploratory Data Analysis (EDA)
- [x] Task 3 – Recommendation System Analysis
- [ ] Task 4 – Trend Prediction Analysis
- [ ] Task 5 – Machine Learning Classification Model
- [ ] Task 6 – Data Science Business Insights Dashboard

## Completed Work

### Task 1 – Data Cleaning & Preprocessing

- Inspected the dataset structure, data types, and content values.
- Replaced unavailable country and director values labeled `Not Given` with `Unknown`, without statistical imputation.
- Converted `date_added` to datetime.
- Detected duplicate content while excluding the unique `show_id`, then removed duplicate records.
- Validated missing values, duplicates, and data types in the cleaned data.
- Saved the cleaned dataset as `netflix_cleaned.csv`.

### Task 2 – Exploratory Data Analysis

Explored the following topics through visualizations and written insights:

- Movies vs TV Shows distribution
- Top countries and Netflix categories
- Release-year trends and rating distribution
- Titles added over time
- Movie duration and TV Show season analysis

### Task 3 – Recommendation System Analysis

Built an exploratory content-based recommendation system using `listed_in`, `director`, `type`, and `rating`.

- Preprocessed director text by removing spaces and converting names to lowercase, so full names behave as meaningful tokens and shared surnames do not create misleading similarity.
- Applied **TF-IDF Vectorization** and **Cosine Similarity** to compare content features.
- Created a reusable `recommendation_system()` function with case-insensitive title matching and a configurable recommendation count.
- Handled and tested the edge case of a title that is absent from the dataset.
- Evaluated recommendation quality using **Category Overlap** (category intersection divided by union) and **Same Content Type Rate** (the percentage of recommendations matching the selected title's content type).

These are **metadata-based evaluation metrics, NOT classification accuracy**. The dataset does not include user ratings or interaction history, so this remains an exploratory recommendation system based on content similarity.

## Technologies Used

- **Programming and data analysis:** Python, Pandas, NumPy
- **Visualization:** Matplotlib, Seaborn
- **Recommendation methods:** Scikit-learn, TF-IDF, Cosine Similarity
- **Development tools:** Google Colab, Git, GitHub

## Repository Files

| File | Description |
| --- | --- |
| [Netflix_Data_Science_Internship.ipynb](Netflix_Data_Science_Internship.ipynb) | Main project notebook containing Tasks 1–3. |
| [Dataset.csv](Dataset.csv) | Original/raw dataset. |
| [netflix_cleaned.csv](netflix_cleaned.csv) | Cleaned dataset generated during Task 1. |

## Run the Project

[![Open in Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/ahmedmamdouh111/Netflix-data-science-internship/blob/main/Netflix_Data_Science_Internship.ipynb)

1. Open the notebook in Google Colab using the badge above.
2. Connect to a Python runtime.
3. Select **Runtime → Run all** to execute the notebook in order.

The notebook loads the raw `Dataset.csv` directly from its GitHub raw file URL, so no manual repository cloning or dataset upload is required. Task 1 writes `netflix_cleaned.csv` to the Colab runtime.

## Project Status

**Work in Progress**

Tasks 1–3 are currently completed. Tasks 4–6 will be added as the internship progresses, and this README will continue to be updated.

## Author

**Ahmed Mamdouh**

[GitHub](https://github.com/ahmedmamdouh111) · [LinkedIn](https://linkedin.com/in/ahmed-mamdouh831)
