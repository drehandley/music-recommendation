# Music Recommendation Algorithm — Project Report

**Project:** Audio Recommendation Algorithm  
**Dataset:** Music Dataset: Lyrics and Metadata from 1950–2019  
**Model:** KMeans Clustering  
**Author:** Dre Handley  

---

## Question 1: EDA Insights — What did you find? Were any columns highly correlated?

Going through the data, a few things stood out right away.

Genre distribution is pretty uneven — pop and country make up the bulk of the dataset (~7,000 and ~5,400 songs respectively), while hip hop only has ~900. That imbalance is worth keeping in mind since it means clusters could end up heavily pop-weighted.

Most of the lyrical score features (dating, violence, romantic, sadness, etc.) are heavily right-skewed. The majority of songs score near zero on most themes, with a small subset that scores high. This makes sense — most songs don't hit every theme hard.

The most interesting bivariate finding was how genre shapes lyrical content. Hip hop songs scored significantly higher on obscene and violence compared to every other genre. Blues and country leaned toward sadness. Jazz had higher music-about-music scores. These patterns make intuitive sense and gave good signal that clustering could actually pick up on real differences.

On the correlation side — no two features had a correlation above 0.7. Nothing was redundant enough to drop purely based on multicollinearity. The heatmap confirmed the features were generally independent of each other.

---

## Question 2: How did you decide which columns to drop or keep?

The dropped columns were straightforward:

- **`Unnamed: 0`** — just the CSV row index, no information
- **`lyrics`** — raw text, not usable for a numeric model (kept separate for optional Word2Vec)
- **`artist_name` and `track_name`** — identifiers, not predictive features
- **`release_date`** — the `age` column already encodes this numerically

All 17 numeric lyrical score features were kept since no high correlations were found in EDA. `genre` and `topic` were kept as reference columns to interpret clusters after the fact — they were not fed into the model.

---

## Question 3: What was the optimal number of clusters? How did you determine it?

**The optimal number of clusters was k=8.**

Two methods were used to find this:

**Elbow Method:** Plotting inertia (sum of squared distances from each point to its cluster center) against k from 2 to 11. The curve flattened noticeably around k=7-9, suggesting diminishing returns beyond that range.

**Silhouette Score:** Measures how well each point fits its own cluster compared to other clusters. Higher is better. Scores by k:

| k | Silhouette Score |
|---|---|
| 2 | 0.0946 |
| 4 | 0.1181 |
| 6 | 0.1531 |
| 7 | 0.1543 |
| **8** | **0.1649** |
| 9 | 0.1641 |
| 10 | 0.1587 |

k=8 gave the highest silhouette score at 0.1649, and the inertia curve had already started to flatten by that point. Both methods agreed, so k=8 was selected.

---

## Question 4: Describe the clusters in human terms

| Cluster | Size | Dominant Genre | Key Characteristics | Human Description |
|---|---|---|---|---|
| 0 | 4,026 | Pop, Country | High world/life score, mid-age songs | **Reflective/life-themed** — songs about everyday life and the world, skewing classic pop and country |
| 1 | 3,834 | Pop, Hip Hop | Highest obscene score (0.45), longer lyrics (avg 122 words) | **Explicit/rap-influenced** — longer, more provocative lyrics; hip hop heavily represented |
| 2 | 4,403 | Rock, Pop | High violence score (0.43), newer songs | **Hard/aggressive** — darker lyrical content, rock-heavy, more recent releases |
| 3 | 1,301 | Country, Blues | Low scoring across most themes, older songs | **Sparse/classic** — shorter, simpler lyrics; older blues and country that don't fit neatly into a theme |
| 4 | 1,732 | Country, Pop | High music score (0.36), older songs | **Music about music** — songs that reference music, instruments, performance; older classics |
| 5 | 4,508 | Pop, Country | Highest sadness score (0.44) | **Melancholic/emotional** — the heartbreak cluster; pop and country ballads that lean heavily sad |
| 6 | 1,524 | Pop, Country | High night/time score (0.33) | **Nightlife/time-themed** — songs about going out, late nights, or the passage of time |
| 7 | 1,332 | Pop, Country | High romantic score (0.31), oldest songs | **Classic romance** — older love songs; the most romantic cluster by score |

---

## Question 5: Test set cluster assignments — which songs would you recommend?

Test songs and their assigned clusters:

| Song | Artist | Genre | Cluster | Cluster Theme |
|---|---|---|---|---|
| Immune | Godsmack | Rock | 0 | Reflective/life-themed |
| Second Chance | Dennis Brown | Reggae | 6 | Nightlife/time-themed |
| Sister Luck | The Black Crowes | Pop | 3 | Sparse/classic |
| Your Cheating Heart | Jerry Lee Lewis | Pop | 5 | Melancholic/emotional |
| Eso Beso | Paul Anka | Pop | 7 | Classic romance |
| Silencio | Noro Morales | Jazz | 2 | Hard/aggressive |
| Pistol Grip Pump | Rage Against the Machine | Rock | 1 | Explicit/rap-influenced |
| Railway and Gun | Taste | Blues | 5 | Melancholic/emotional |
| Messin' With My Mind | Randy Travis | Country | 6 | Nightlife/time-themed |
| Playing God | Paramore | Pop | 2 | Hard/aggressive |

**Recommendation logic:** Songs in the same cluster share similar lyrical themes and emotional tone. If a user is listening to *Your Cheating Heart* (Cluster 5 — Melancholic), the algorithm would recommend other songs in that cluster — heartbreak-heavy pop and country like *Railway and Gun* by Taste, which also landed in Cluster 5. Similarly, a user who listens to *Pistol Grip Pump* (Cluster 1 — Explicit/aggressive) would get recommendations from that cluster which skews hip hop and harder rock.

