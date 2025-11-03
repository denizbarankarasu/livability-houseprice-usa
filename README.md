# livability-houseprice-usa
1) Problem & Goal

We aim to predict house prices in King County (Seattle area) and study how neighborhood livability relates to price.
Key idea: enrich each home with nearby schools, hospitals, and gun-violence incidents to compute:

Proximity: distance to the nearest point (km)

Density: counts within 1/3/5 km

LivabilityScore (0–100): Safety (40%) + Schools (30%) + Hospitals (30%)

Research questions
Do proximity/density features reduce RMSE vs. a house-only baseline?
Which livability components (safety, schools, hospitals) show the strongest association with price?

2) Datasets
House Sales in King County, USA (harlfoxem) — base target/features
https://www.kaggle.com/datasets/harlfoxem/housesalesprediction
US Schools Dataset (andrewmvd) — school locations
https://www.kaggle.com/datasets/andrewmvd/us-schools-dataset
USA Hospitals (carlosaguayo) — hospital locations
https://www.kaggle.com/datasets/carlosaguayo/usa-hospitals
Gun Violence Incidents in the USA (emmanuelfwerr) — safety proxy
https://www.kaggle.com/datasets/emmanuelfwerr/gun-violence-incidents-in-the-usa

4) Planned Methods (Step-2)
Feature building: nearest distance + counts within 1/3/5 km; compute LivabilityScore (weights configurable).
Models: Ridge/Lasso/RandomForest with 5-fold CV (RMSE).
Comparison: Baseline (house-only) vs. Enriched (baseline + proximity/density + score).
Reporting: CV RMSE tables, feature importance (RF importances, Ridge/Lasso coefficients), brief error analysis.

5) Ethics & Limitations
Coverage bias: incident reporting varies across places/time—treat “safety” as exploratory.
Privacy: no PII; only public point datasets and property attributes.
