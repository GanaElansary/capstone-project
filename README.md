# capstone project


## Topic  Mood-based clustering (unsupervised)
Without using genre labels at all, can songs be grouped into meaningful 'mood' clusters based on their audio DNA? This project applies unsupervised clustering (K-Means/DBSCAN) to audio features to discover natural groupings of tracks, then interprets each cluster's dominant characteristics  e.g., 'high energy + low valence = intense/dark'  as a foundation for a mood-based playlist generator.

**Status:** In progress

## Dataset

`spotify_songs.csv` — Spotify track metadata and audio features (Kaggle: "30000 Spotify Songs").

## Audio "DNA" Features

The clustering input is built from audio features only — no genre, popularity, or metadata:

`energy` · `valence` · `danceability` · `acousticness` · `loudness` · `tempo` · `mode`

`energy` and `valence` form the core mood axes (valence–arousal model); the rest add texture and intensity.

## Approach

1. **Clean & explore** — de-duplicate tracks, check for outliers (found in `acousticness`, `loudness`, `tempo`) and missing values.
2. **Scale** — compared `RobustScaler`, `StandardScaler`, and `MinMaxScaler`; **`StandardScaler`** was selected as final, outperforming the others on silhouette score, Davies-Bouldin index, and cluster valence consistency.
3. **Cluster** — compared **K-Means**, **DBSCAN**, and **Agglomerative Clustering** at K=4. **K-Means was confirmed best**: Agglomerative and DBSCAN only recovered ~2 real density-based clusters plus noise fragments, while K-Means produced four well-separated, evenly-sized clusters. K-Means also natively supports `.predict()`, which the single-track workflow needs.
4. **Choose K** — swept K=2–10 using the Elbow method, Silhouette Score, and Davies-Bouldin Index. The statistically optimal K is a starting point, not the final answer — it's weighed against whether the resulting clusters are interpretable as distinct, useful moods.
5. **Interpret clusters** — label each cluster from its centroid's energy/valence position *relative to the other clusters*, e.g. high energy + low valence → Intense/Dark, high energy + high valence → Upbeat/Hype.
6. **Single-track workflow** — given a new track, scale its features, predict its cluster, compare it to the cluster's average profile (radar chart), visualize its location in 2D (PCA), and generate a mood-matched playlist from its nearest neighbors in the same cluster.

## Status & Next Steps

- [ ] Export the fitted scaler, K-Means model, and PCA artifacts (`.joblib`)
- [ ] Wrap the pipeline in a backend class for reuse
- [ ] Build a UI (Streamlit/Gradio) for the interactive single-track demo — predict → radar chart → mood-matched playlist

## Tech Stack

Python · pandas · scikit-learn · matplotlib


