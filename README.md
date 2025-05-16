Music Recommendation via Unsupervised Clustering of Spotify Audio Features
Team 13
Abstract
Streaming services host well over 100 million tracks, yet their recommender pipelines are opaque to outside researchers. We build a lightweight, fully transparent prototype that clusters 114 k Spotify tracks on 21 low level audio features and uses Euclidean proximity within that feature space to recommend songs. On a held out set of synthetic “playlists” the cluster filtered search matches the relevance of a whole library k nearest neighbors (K NN) baseline while evaluating only ≈ 15 % of the catalogue, suggesting that unsupervised clustering can accelerate content based recommendation without loss of quality. 

Motivation & Research Questions
Listeners are overwhelmed by choice; even expert users struggle to sift through millions of tracks. Commercial recommenders (e.g. Spotify Radio) work well but are proprietary black boxes. By recreating one building block of such systems with open data we ask:
Can unsupervised clustering of track audio features effectively produce relevant and satisfying music recommendations?

Literature Review
Early academic systems relied on collaborative filtering, more recent work blends audio content with user context. Pichl et al. introduce context aware playlist generation: songs are first clustered on audio features, then ordered via graph traversal conditioned on user mood and activity. We adopt their clustering insight but deliberately exclude contextual signals in order to isolate the value of audio features alone. 
Our contribution differs by (i) benchmarking cluster filtered search against a whole library K NN on an open 100 k track dataset and (ii) publishing fully reproducible code in a single notebook.

Data
Property	Value
Source	Spotify Tracks Dataset on Kaggle  

Size	114 ,000 tracks, 125 genres
Features (21)	tempo, energy, danceability, valence, loudness, acousticness, instrumentalness, liveness, speechiness…
Format	CSV 

Method
Pre processing
1.	Train/Test Split: 80/20
2.	Scaling: All numeric attributes were min max scaled to [0,1].
Clustering
 
Algorithm: K-means clustering algorithm, k = 7 (silhouette analysis and elbow criterion).
 
Cluster labels are appended to every track.
Recommendation Logic
Given a seed playlist S (10 tracks) and a candidate pool C:
1.	Compute centroid of S in feature space.
2.	Measure Euclidean distance
3.	Return the 10 closest unseen tracks.
 
Evaluation Protocol
•	Test set: 10 synthetic playlists (random 10 song samples).
•	Metric: mean Euclidean distance between each output track and its seed centroid (lower = better).
 
GitHub repository: https://github.com/ishraqsadik/CAP4773-SMM-Team13 

Results
   
For every test playlist, the cluster-filtered recommender and the full-library K-NN produced almost identical song lists. Their average distance to the seed-playlist “sound” never rose above 0.92, while the random baseline hovered between 2.26 and 3.46. The smarter recommenders stayed close to the vibe of the originals, while random picks drifted far off. Because clustering trims the search space by roughly 85% yet keeps the same quality scores, it’s a fast, lightweight step that doesn’t sacrifice recommendation accuracy.
Discussion
Our findings show that you don’t need the heavy, context-aware machinery of Pichl et al. to deliver sensible song suggestions. With nothing but 21 raw audio features we first clustered the ≈114 k-track catalogue, then ran K-nearest-neighbour (K-NN) search inside the dominant cluster of each seed playlist. That lightweight shortcut examined only about 15 % of the library, yet its average distance to the playlist’s “sound centroid” (0.47 – 0.92 across playlist types) was essentially identical to a full-library K-NN scan. In other words, a single unsupervised clustering pass can slash the candidate pool while leaving recommendation quality untouched—a pragmatic win for anyone who wants faster, cheaper content-based filtering.
There are, however, clear caveats. Our model knows nothing about a listener’s mood, location, or listening history; it sees only tempo, energy, and similar frame-level descriptors. The evaluation playlists are synthetic samples rather than real usage logs, and the underlying dataset skews toward pre-2023 tracks, meaning both popularity shifts, and newly emerging genres are under-represented. These factors limit how confidently we can generalize the results to live streaming platforms.
Next, we plan to seed the system with genuine Spotify playlists to check whether clustering still keeps pace—or even beats—naïve K-NN when faced with real-world taste profiles. A second milestone is a head-to-head comparison with Spotify’s own recommendation API, providing a practical benchmark of user‐perceived quality. Finally, we’ll move beyond “audio-only” by blending in lightweight context such as release year, genre tags, or session-level signals (e.g., time of day), turning the current proof-of-concept into a more versatile hybrid recommender.





References
Pichl, M., Zangerle, E., & Specht, G. (2018). Context Aware Playlist Generation Using Pre Computed Audio Features.
Team Member Contributions
Member	Responsibilities
Ishraq Khan	Statistical analysis and playlist generation, including visualizations
Farhan Shahriar Abrar	Built recommendation functions
Aanthoni Dsouza	Data acquisition & cleaning; Idea generation
Efaz Ibrahim	Implemented K Means algorithm
Isfahan Jawad Juboraj	Research analysis and motivation



