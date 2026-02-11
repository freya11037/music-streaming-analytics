# Data

This folder contains two datasets used across the three analysis phases. **Neither file is tracked by Git** (both are listed in `.gitignore`) due to file size. Instructions for obtaining each are below.

---

## Files

| File | Size | Description |
|------|------|-------------|
| `spotify_tracks.csv` | ~113K rows, 28 features | Raw dataset from Hugging Face |
| `spotify_cleaned.csv` | ~113K rows | Cleaned version produced by Phase 1 EDA |

---

## `spotify_tracks.csv` — Raw Data

**Source:** [Spotify Tracks Dataset on Hugging Face](https://huggingface.co/datasets/maharshipandya/spotify-tracks-dataset)  
**Tracks:** 113,393  
**Features:** 28 (audio features, metadata, popularity score)  
**Key features include:** `popularity`, `danceability`, `energy`, `loudness`, `speechiness`, `acousticness`, `instrumentalness`, `liveness`, `valence`, `tempo`, `duration_ms`, `track_genre`

### How to get it

**Option A — Auto-download (recommended):**  
Simply run `01_Music_EDA.ipynb`. The notebook will automatically download the dataset from Hugging Face and save it here as `spotify_tracks.csv`.

**Option B — Manual download:**  
```python
from datasets import load_dataset
import pandas as pd

ds = load_dataset("maharshipandya/spotify-tracks-dataset")
df = ds['train'].to_pandas()
df.to_csv('data/spotify_tracks.csv', index=False)
```

---

## `spotify_cleaned.csv` — Cleaned Data

**Source:** Produced by `notebooks/01_Music_EDA.ipynb`  
**Cleaning steps applied:**
- Removed duplicate tracks
- Handled missing values
- Filtered outliers in audio features
- Standardized genre labels
- Added `is_hit` binary column (top 30% popularity threshold)

### How to get it

Run `01_Music_EDA.ipynb` from top to bottom. The cleaned file is saved automatically at the end of the notebook. Notebooks 02 and 03 both load from this file.

---

## Dependency Order

```
Hugging Face API
      ↓
spotify_tracks.csv        ← created by: 01_Music_EDA.ipynb
      ↓
spotify_cleaned.csv       ← created by: 01_Music_EDA.ipynb
      ↓
Used by: 02_Genre_Specific_Regression.ipynb
         03_Hit_Prediction_Models.ipynb
```

---

*To reproduce the full analysis, only `01_Music_EDA.ipynb` needs to be run first. Notebooks 02 and 03 can then be run in any order.*
