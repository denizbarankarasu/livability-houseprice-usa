# livability-houseprice-usa
Livability-Enhanced House Price Modeling (King County + US Schools / Hospitals / Crime)

This project geographically enriches the House Sales in King County (Seattle area) dataset with US Schools, USA Hospitals, and Gun Violence Incidents (Kaggle). For each home, within 1/3/5 km we compute:

Proximity: distance (km) to the nearest school/hospital

Density: counts of schools/hospitals/incidents within the radius

LivabilityScore: a 0–100 score combining Safety (40%), Schools (30%), Hospitals (30%)

We then compare baseline (house features only) vs. enriched (adds proximity/density + LivabilityScore) models using 5-fold CV, RMSE.
