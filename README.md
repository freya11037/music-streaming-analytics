# Music Streaming Analytics: What Makes a Song a Hit?

**Discovering Simpson's Paradox in Spotify Data — And Why Genre Changes Everything**

## The Question

Can we predict whether a song will be popular based on how it sounds? Spotify provides audio features for every track — danceability, energy, acousticness, tempo, and more. Intuitively, there should be a formula: certain combinations of these features should produce hits. We set out to find it.

## The Surprising Answer

**There is no universal hit formula.** But the reason *why* is more interesting than the answer itself.

When we analyzed 113,000+ Spotify tracks across 114 genres, we discovered **Simpson's Paradox** — a statistical phenomenon where a trend that appears in aggregated data reverses or disappears when the data is split into subgroups.

Specifically: audio features like danceability and energy show almost no relationship with popularity when you look at all songs together. But within individual genres, these same features are strong predictors — and often in **opposite directions.** High danceability predicts hits in pop but not in classical. High acousticness hurts popularity in hip-hop but helps in folk. When you pool everything together, these opposite effects cancel out, creating the illusion that audio features don't matter.

## Key Results

| Phase | Finding | Evidence |
|-------|---------|----------|
| **EDA** | Audio-popularity correlations appear near zero overall, but are strong within genres | Simpson's Paradox identified through genre-stratified analysis |
| **Regression** | Genre context produces a 13× improvement in explanatory power | R² = 0.023 (pooled) → R² = 0.313 (with genre interactions) |
| **Classification** | Genre-aware ML models significantly outperform audio-only models | AUC = 0.75 (audio only) → AUC = 0.87 (with genre) |

The finding held up across every method — correlations, OLS regression, logistic regression, random forest, and XGBoost.

## Project Structure

```
├── 01_EDA_Exploration.ipynb          # Phase 1: Exploratory data analysis
├── 02_Genre_Regression_Analysis.ipynb # Phase 2: Genre-specific regression
├── 03_Hit_Prediction_Models.ipynb     # Phase 3: ML classification
├── data/
│   └── spotify_tracks_cleaned.csv     # 113,393 tracks, 28 features                      
├── Project_Summary.md                 # Non-technical 2-page summary
└── README.md
```

## Notebooks

### Notebook 01 — Exploratory Data Analysis
Explores the dataset, identifies the Simpson's Paradox phenomenon, defines the binary hit target (top 30% popularity), and conducts genre-level analysis including the explicit content paradox (where explicit songs appear more popular overall due to genre confounding).

### Notebook 02 — Genre-Specific Regression
Quantifies the genre effect through three regression approaches: aggregate OLS (R² = 0.023), genre-specific models (average R² = 0.080), and a feature × genre interaction model (R² = 0.313). Demonstrates that feature effects like danceability coefficients range from -0.3 to +0.4 across genres.

### Notebook 03 — Hit Prediction Models
Tests three hypotheses using ML classification:
- **H1:** Linear audio-feature models are weak predictors (LR AUC = 0.61) — but non-linear models recover moderate signal (XGBoost AUC = 0.75) through feature interactions
- **H2:** Genre context significantly improves prediction (XGBoost AUC jumps to 0.87) — but only for models that can learn interactions, not linear models
- **H3:** Per-genre models show that feature importance rankings differ across genres, confirming there is no universal hit formula

## Methodology

- **Dataset:** Spotify Tracks from Hugging Face (113,393 tracks, 114 genres, 28 features)
- **Target variable:** `is_hit` — binary classification using the 70th percentile of popularity as the threshold
- **Statistical methods:** ANOVA, Cohen's d effect sizes, OLS regression with interaction terms
- **ML models:** Logistic Regression, Random Forest, XGBoost (with class imbalance handling via balanced weights)
- **Validation:** 5-fold stratified cross-validation confirms all results are stable (low fold-to-fold variance)

## Practical Implications

1. **For music recommendation systems:** Genre-aware models don't just perform better — they perform better because genre changes *which* features matter, not just *how much* they matter. Models that treat genre as a simple filter leave significant signal on the table.

2. **For the music industry:** Audio-based hit prediction is viable for some genres (where audio conventions are strong) but fundamentally limited for others (where popularity is driven by marketing, artist fame, or cultural timing).

3. **For data science practice:** Simpson's Paradox isn't just a textbook concept. In this dataset, ignoring it led to the conclusion that audio features are useless (R² = 0.023). Accounting for it revealed moderate predictive power (AUC = 0.87). Subgroup analysis should be standard practice.

## Limitations

- Popularity reflects Spotify streaming metrics (influenced by playlists, marketing, recency), not song quality
- Audio features are incomplete — lyrics, artist fame, and marketing are absent
- Genre labels are assigned, not emergent — many songs span multiple genres
- Hyperparameters use reasonable defaults; formal tuning could improve absolute performance but would not change relative findings

## Tools

Python, pandas, NumPy, scikit-learn, XGBoost, statsmodels, matplotlib, seaborn

## Author

Linh Diep — MS Business Analytics, University of Texas at Arlington

February 2026
