## ReelMetrics

**A Data-Driven Movie Analytics + Success Prediction System**

Analyze how movies perform across **1995–2024** using IMDb-style metadata + TMDb popularity signals, then train ML models to **classify movies into outcome tiers** (*Failure / Mediocre / Successful*) based on financial + reception features.

## Links

| | Link |
|---|---|
| **Project website** | [https://sites.google.com/view/reel-metrics/introduction](https://sites.google.com/view/reel-metrics/introduction) |

**Quick try:** open `ML_Project_4.ipynb` → run all cells → scroll to the **model training + evaluation** sections (SVM / PCA / XGBoost) and the visualizations.

## Problem Statement

Modern movie success is multi-factorial and hard to evaluate (or predict) with a single metric:

- **Revenue alone is insufficient.** Performance depends on budget, audience reception, awards traction, and momentum.
- **Signals are scattered across sources.** Popularity and audience response live in different ecosystems (e.g., TMDb vs. IMDb-style datasets).
- **Stakeholders need actionable insight.** Understanding correlations helps guide marketing, production, and distribution decisions.
- **Trends shift over time.** The past 3 decades include major market changes (including streaming era effects), requiring longitudinal analysis.

ReelMetrics consolidates historical movie metadata into a single analytics workflow and applies machine learning to categorize outcomes and surface patterns.

## Business Impact

| Stakeholder | Value Delivered |
|---|---|
| **Producers / studios** | Identify performance drivers (budget, rating, nominations, popularity) and de-risk greenlight decisions. |
| **Marketers** | Understand what correlates with success to optimize positioning and campaign timing. |
| **Streaming platforms** | Use feature patterns to support acquisition and catalog strategy decisions. |
| **Film analysts / enthusiasts** | Explore trends by genre, ratings, and time period using reproducible analysis. |
| **Data science learners** | End-to-end case study: ingestion → cleaning → EDA → modeling → evaluation. |

## Architecture

```
TMDb API (Top movies/year) ─┐
                            ├─→ Data Merge + Cleaning (1995–2024)
IMDb-style CSV files ───────┘
            │
            ▼
EDA + Visualization (Plotly / Seaborn / Matplotlib)
            │
            ▼
Feature Engineering + Labeling (MOVIE_CATEGORY)
            │
            ▼
Modeling
  - SVM (Linear / RBF / Poly)
  - PCA experiments
  - XGBoost classifier
            │
            ▼
Evaluation (accuracy, confusion matrix, classification report)
```

## Tech Stack

| Layer | Technology |
|---|---|
| Data ingestion | Python, `requests`, TMDb API |
| Data wrangling | `pandas`, `numpy` |
| Visualization | `matplotlib`, `seaborn`, `plotly` |
| Modeling | `scikit-learn` (SVM, scaling, PCA), `xgboost` |
| Environment | Jupyter / notebook workflow |

## Results

The pipeline produces a consolidated movie dataset (1995–2024), runs EDA, and trains classifiers to predict a 3-class outcome label (`MOVIE_CATEGORY`: `Failure`, `Mediocre`, `Successful`) using financial + reception features.

### Dataset Snapshot (from `ML_Project_4.ipynb`)

- **Rows**: 15,850 movies
- **Target label**: `MOVIE_CATEGORY` (3 classes)
- **Class distribution**:
  - `Failure`: 10,460
  - `Successful`: 4,204
  - `Mediocre`: 1,186
- **Model feature set**:

`['DURATION', 'RATING', 'VOTES', 'BUDGET', 'GROSSWORLDWIDE', 'NOMINATIONS', 'OSCARS']`

### Model Metrics (from notebook runs)

#### SVM (scaled features, 80/20 split)

- **Linear SVM (LinearSVC)**:
  - `C=0.01`: **0.8328**
  - `C=0.1`: **0.8319**
  - `C=1`: **0.8315**

- **RBF SVM (SVC)**:
  - `C=0.01`: **0.8180**
  - `C=1`: **0.8454**
  - `C=10`: **0.8580** *(best SVM in the notebook summary)*

#### XGBoost (stratified 80/20 split)

- **Accuracy**: **0.9852**
- **Classification report (test set, support=3170)**:
  - `Failure`: precision **0.99**, recall **0.99**, f1 **0.99**
  - `Mediocre`: precision **0.92**, recall **0.92**, f1 **0.92**
  - `Successful`: precision **0.99**, recall **0.98**, f1 **0.98**

- **XGBoost hyperparameters (as configured in notebook)**:
  - `max_depth=6`, `learning_rate=0.1`, `n_estimators=200`,
    `subsample=0.8`, `colsample_bytree=0.8`

> Note: These scores come from the notebook’s specific preprocessing and label encoding steps.

## Setup Instructions

### 1. Clone and set up environment

```powershell
git clone https://github.com/arushijha03/reelMetrics.git
cd reelMetrics

python -m venv venv
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
.\venv\Scripts\activate
```

### 2. Install dependencies

```powershell
pip install -r requirements.txt
```

### 3. TMDb API key (optional, for data collection)

Some notebooks call the TMDb “discover” endpoint to fetch popular movies by year.

- **TMDb**: [https://www.themoviedb.org/settings/api](https://www.themoviedb.org/settings/api)

Windows:

```powershell
setx TMDB_API_KEY "YOUR_KEY_HERE"
```

In Python (recommended):

```python
import os
api_key = os.getenv("TMDB_API_KEY")
```

### 4. Run notebooks

```powershell
jupyter notebook
```

Suggested order:

- `ML_Project.ipynb` (data ingestion + merging)
- `ML_Project_Two.ipynb`, `ML_Project_Three.ipynb` (analysis iterations)
- `ML_Project_4.ipynb` (full pipeline + modeling + final metrics)

### 5. Data notes

The notebook workflow expects:

- TMDb API access for “top popular movies per year” (optional)
- Local CSVs matching `merged_movies_data_*.csv` for the IMDb-style dataset merge

If you don’t have the CSV exports locally, run the notebooks that apply, then plug in your dataset in the same column format used by `ML_Project_4.ipynb`.

## Demo Checklist

1. Open `ML_Project_4.ipynb` and run all.
2. Show merged dataset schema and the feature list used for modeling.
3. Show EDA plots (ratings vs gross, budget vs gross, trends by year).
4. Show SVM results (accuracy + confusion matrix + classification report).
5. Show XGBoost results (accuracy + classification report).

## Testing

This project is currently notebook-driven and does not ship an automated `pytest` suite. The recommended “test” is to rerun `ML_Project_4.ipynb` end-to-end and confirm the reported accuracies and classification reports.

## Deployment

ReelMetrics is currently presented as:

- a project website (Google Sites)
- a reproducible notebook pipeline (GitHub)

If you want, it can be upgraded into a small app (e.g., Streamlit dashboard + prediction endpoint).

## 4-Month Build Plan

| Month | Focus |
|---|---|
| 1 | Define the problem + success metrics; collect initial data sources (TMDb + IMDb-style exports); set up notebook scaffolding |
| 2 | Data cleaning + consolidation (1995–2024), feature engineering, EDA + trend analysis |
| 3 | Modeling iteration (SVM variants, PCA experiments), evaluation via confusion matrices + classification reports |
| 4 | XGBoost modeling + tuning, finalize results, polish visuals/storytelling, publish the project website, and document reproducible steps in the README |

## Project Structure

```
reelMetrics/
├── ML_Project.ipynb
├── ML_Project_Two.ipynb
├── ML_Project_Three.ipynb
├── ML_Project_4.ipynb
├── requirements.txt
└── README.md
```

## License

MIT
