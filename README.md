# Music Recommendation Algorithm

Unsupervised machine learning pipeline that clusters songs by lyrical content to power a recommendation engine.

**Dataset:** 28,362 songs from 1950–2019 with lyrical theme scores  
**Model:** KMeans Clustering (k=8)  
**Libraries:** scikit-learn, pandas, numpy, matplotlib, seaborn

---

## Project Structure

```
music-recommendation/
├── notebooks/
│   ├── 01_eda.ipynb              # Exploratory data analysis
│   ├── 02_preprocessing.ipynb    # Cleaning, scaling, PCA
│   └── 03_modeling.ipynb         # KMeans, tuning, test predictions
├── data/
│   ├── train.csv                 # Raw training data (gitignored)
│   ├── test.csv                  # Raw test data (gitignored)
│   ├── train_processed.csv       # Scaled + PCA reduced (gitignored)
│   ├── train_clustered.csv       # Training data with cluster labels (gitignored)
│   └── test_clustered.csv        # Test predictions (gitignored)
├── docs/
│   └── report.md                 # Project report with analysis answers
└── README.md
```

## How to Run

1. Place `train.csv` and `test.csv` in the `data/` folder
2. Run notebooks in order: `01_eda` → `02_preprocessing` → `03_modeling`
3. Each notebook saves output CSVs that the next notebook reads

## Results Summary

- **Optimal clusters:** k=8 (silhouette score: 0.1649)
- **PCA components used:** 14 (capturing 90% of variance)
- Clusters represent distinct lyrical themes: melancholic, explicit, romantic, aggressive, nightlife, music-about-music, reflective, and sparse/classic

