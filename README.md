# Music Recommendation via Unsupervised Clustering of Spotify Audio Features

*Team 13 – CAP 4773 Social Media Mining (University of South Florida)*  

---

## Abstract

Streaming platforms expose listeners to catalogues exceeding 100 million tracks, yet their proprietary recommender pipelines remain opaque to researchers. We present a fully transparent prototype that clusters ≈ 114 k Spotify tracks on 21 low‑level audio features and recommends songs via Euclidean proximity within that space. When evaluated on synthetic test playlists, our **cluster‑filtered search** matches the relevance of a brute‑force *k*-nearest‑neighbours (K‑NN) scan while examining only ≈ 15 % of the catalogue—evidence that a single unsupervised pass can accelerate content‑based recommendation without degrading quality.

---

## Table of Contents
1. [Motivation & Research Questions](#motivation--research-questions)  
2. [Literature Review](#literature-review)  
3. [Dataset](#dataset)  
4. [Methodology](#methodology)  
5. [Results](#results)  
6. [Discussion](#discussion)  
7. [Limitations](#limitations)  
8. [Next Steps](#next-steps)  
9. [Quick Start](#quick-start)  
10. [Repository Structure](#repository-structure)  
11. [Team Contributions](#team-contributions)  
12. [References](#references)  
13. [License](#license)

---

## Motivation & Research Questions

Listeners face information overload; even expert users cannot manually sift millions of tracks. Commercial engines (e.g. Spotify Radio) work well but are black boxes. We therefore ask:

> **Can unsupervised clustering of raw audio features alone deliver relevant music recommendations comparable to a full‑library K‑NN baseline?**

---

## Literature Review

Early academic work leaned on collaborative filtering, while recent studies blend audio content with rich user context. Pichl et al. first clustered tracks on audio features before re‑ordering them via context‑aware graph traversal. We adopt their clustering insight but *deliberately remove* contextual signals to isolate the predictive power of audio features themselves, benchmarking cluster‑filtered search against a whole‑library K‑NN on an open dataset.

---

## Dataset

| Property | Value |
|----------|-------|
| **Source** | Spotify Tracks Dataset (Kaggle) |
| **Size** | 114 000 tracks, 125 genres |
| **Features (21)** | tempo, energy, danceability, valence, loudness, acousticness, instrumentalness, liveness, speechiness, … |
| **Format** | CSV |

All numeric attributes were min‑max scaled to [0, 1] prior to modelling.

---

## Methodology

### Pre‑processing  
* 80 / 20 train–test split  
* Min‑max scaling on every numeric field  

### Clustering  
* **Algorithm:** *k*-means, *k = 7* (chosen via silhouette analysis & elbow criterion)  
* Cluster label appended to each track record  

### Recommendation Logic  
1. Accept a **seed playlist *S*** (10 tracks).  
2. Compute the centroid of *S* in 21‑dimensional feature space.  
3. Within the **dominant cluster** of *S*, return the 10 unseen tracks with the smallest Euclidean distance to that centroid.  

### Evaluation Protocol  
* **Test set:** 10 synthetic playlists (random 10‑song samples)  
* **Metric:** Mean Euclidean distance between each recommended track and the seed‑playlist centroid (lower = better)  
* **Baselines:**  
  * Whole‑library K‑NN (audio content only)  
  * Random selection  

---

## Results

| Recommender | Mean Distance ↓ | Candidate Pool Size |
|-------------|-----------------|---------------------|
| **Cluster‑filtered K‑NN** | 0.47 – 0.92 | ≈ 15 % |
| Whole‑library K‑NN | 0.45 – 0.91 | 100 % |
| Random | 2.26 – 3.46 | 100 % |

Across all playlists, the cluster‑filtered search retained virtually the same proximity to the desired “sound” as a brute‑force K‑NN, while scanning only a fraction of the catalogue.

---

## Discussion

A single unsupervised clustering pass can **slash the candidate pool by ≈ 85 %** with no measurable loss in recommendation quality—an attractive speed‑up for anyone building transparent, content‑based recommenders. Nevertheless, the model sees only raw audio descriptors and therefore misses important contextual cues (mood, session, user history). Synthetic evaluation playlists and a dataset skewed toward pre‑2023 tracks further limit generalisability.

---

## Limitations

* **Audio‑only perspective:** Ignores user context and listening history.  
* **Synthetic evaluation data:** Not validated on real user playlists.  
* **Dataset bias:** Under‑represents newly released music and emerging genres.  

---

## Next Steps

1. **Seed with genuine Spotify playlists** to test real‑world robustness.  
2. **Benchmark against Spotify’s Recommendation API** as an external gold standard.  
3. **Integrate lightweight context** (e.g. release year, genre tags) to evolve into a hybrid recommender.  

---

## Quick Start

```bash
# Clone the repository
git clone https://github.com/your-username/your-repo.git
cd your-repo

# Create Python environment
python -m venv venv
source venv/bin/activate         # Windows: venv\Scripts\activate
pip install -r requirements.txt

# Launch the end‑to‑end notebook
jupyter notebook notebooks/Spotify_Clustering_Recommendation.ipynb
```

The notebook covers data loading, clustering, recommendation, and evaluation in a single run.

---

## Repository Structure

```
.
├── data/                      # (optional) local cache of CSVs
├── notebooks/
│   └── Spotify_Clustering_Recommendation.ipynb
├── src/
│   ├── preprocessing.py
│   ├── clustering.py
│   └── recommend.py
├── requirements.txt
└── README.md
```

---

## Team Contributions

| Member | Responsibilities |
|--------|------------------|
| **Ishraq Khan** | Statistical analysis & playlist generation; visualisations |
| **Farhan Shahriar Abrar** | Core recommendation functions |
| **Aanthoni Dsouza** | Data acquisition & cleaning; idea generation |
| **Efaz Ibrahim** | K‑means implementation |
| **Isfahan Jawad Juboraj** | Research analysis & motivation |

---

## References

* Pichl, M., Zangerle, E., & Specht, G. (2018). *Context‑Aware Playlist Generation Using Pre‑Computed Audio Features*.  

---

## License

This project is released under the **MIT License**—see the `LICENSE` file for details.

