# Livability & House Prices in the United States

## Abstract
This project aims to examine how various livability indicators—such as crime rate, cost of living, education quality, healthcare access, environmental factors, and local economic conditions—relate to housing prices across the United States. By combining multiple public datasets and analyzing correlations, distributions, and spatial patterns, the project seeks to uncover which livability dimensions have the strongest relationship with home values. The goal is to provide data-driven insights into why certain regions are more expensive and how livability factors contribute to price differences.

## Motivation
I chose this topic because housing affordability and regional livability differences are major socioeconomic issues in the U.S. People often assume that “expensive areas are simply better,” but this relationship is not always straightforward. I want to investigate whether livability metrics actually justify the price differences or if other hidden factors are at play. Understanding these relationships can help individuals make smarter relocation or investment decisions and may also support policy discussions on housing inequality.

## Datasets
House Sales in King County, USA (harlfoxem) — base target/features https://www.kaggle.com/datasets/harlfoxem/housesalesprediction

US Schools Dataset (andrewmvd) — school locations https://www.kaggle.com/datasets/andrewmvd/us-schools-dataset

USA Hospitals (carlosaguayo) — hospital locations https://www.kaggle.com/datasets/carlosaguayo/usa-hospitals

Gun Violence Incidents in the USA (emmanuelfwerr) — safety proxy https://www.kaggle.com/datasets/emmanuelfwerr/gun-violence-incidents-in-the-usa

## Methodology

### 1. Data Cleaning
- Remove irrelevant rows and columns  
- Handle missing values (deletion, mean/median imputation)  
- Fix inconsistent region names or FIPS identifiers  
- Convert categorical fields into usable numerical formats  
- Normalize units where required  

### 2. Data Integration
- Merge livability indicators with house price data using:
  - FIPS codes  
  - County or state names  
  - ZIP-code identifiers  
- Ensure consistent geographic alignment  
- Create a unified analysis-ready dataset  

### 3. Exploratory Data Analysis (EDA)
- Summary statistics (mean, median, variance, distribution checks)  
- Correlation heatmaps (house price vs. livability metrics)  
- Outlier detection  
- State-level vs. national-level comparisons  
- Boxplots and scatterplots for key variables  
- Spatial visualization (U.S. map showing price or livability scores)  

### 4. Planned Analyses
- Hypothesis testing (e.g., t-tests, ANOVA)  
- Regression modeling (simple/multiple linear regression)  
- Clustering (optional)  
- Feature importance analysis  

## Expected Findings (Possible Outcomes)
- Higher-quality education, lower crime rates, and better healthcare access are expected to correlate with higher home prices.
- Some livability metrics (e.g., environment score or cost of living) may show stronger predictive power than others.
- Certain states may deviate from the national trend due to unique economic factors.
- A combined “livability score” may explain a significant portion of house-price variation.

## Tools
- Python  
- Pandas, NumPy  
- Matplotlib, Seaborn, Plotly  
- Scikit-learn  
- GeoPandas (for maps)

## Phase 2 – Data Collection, Exploratory Data Analysis and Hypothesis Testing

### 2.1 Data Collection (Recap)

For this phase I kept the same datasets from Phase 1 and restricted them to *King County, Washington*:

- *House prices:* kc_house_data.csv  
  - 21,613 residential sales in King County.  
  - Used columns: price, bedrooms, bathrooms, sqft_living, sqft_lot, grade, condition, zipcode, lat, long, etc.

- *Public schools:* Public_Schools.csv  
  - Filtered to rows whose ZIP appears in the housing dataset.  
  - Result: 592 public schools in King County ZIP codes.

- *Private schools:* Private_Schools.csv  
  - Same ZIP filtering as above.  
  - Result: 33 private schools in King County ZIP codes.

- *Hospitals:* Hospitals.csv  
  - Filtered to King County ZIP codes.  
  - Result: 33 hospitals.

- *Gun violence incidents:* all_incidents.csv  
  - Filtered to incidents in Washington state and to a set of King County cities  
    (Seattle, Kent, Federal Way, Renton, Auburn, Bellevue, etc.).  
  - Result: 1,706 incidents.  
  - This dataset does *not* contain ZIP codes or coordinates, so it is analyzed
    separately and is not merged with the house price dataset.

After filtering, I aggregated school and hospital counts at ZIP level and merged them
into the housing data:

- public_school_count – number of public schools in each ZIP  
- private_school_count – number of private schools in each ZIP  
- hospital_count – number of hospitals in each ZIP  

The final merged table king_county_merged.csv contains house-level observations
with these livability indicators attached.

---

### 2.2 Exploratory Data Analysis (EDA)

*House prices*

- Price distribution is right-skewed with most homes clustered in the lower to
  mid-price range and a small number of very expensive properties.
- sqft_living, bathrooms, bedrooms and grade show positive relationships
  with price, as expected.
- A correlation matrix confirms that sqft_living and grade are among the
  strongest structural predictors of price.

*Livability indicators*

Using the merged dataset, I explored how livability indicators vary by ZIP:

- public_school_count is higher in denser, more urban ZIP codes.
- private_school_count is concentrated in a smaller number of ZIP codes,
  typically wealthier areas.
- hospital_count is low overall (0–3 per ZIP) and concentrated around major
  urban centers.

*Incidents (separate descriptive analysis)*

Because the incident dataset cannot be merged by ZIP, I performed a separate EDA:

- *Incidents by city:* Seattle has by far the highest number of incidents, followed
  by Kent, Federal Way, Renton and Auburn. Smaller cities in King County have
  much lower incident counts.
- *Incidents over time:* Annual incident counts peak around 2015–2017 and tend
  to decline in the most recent years.

This provides high-level safety context for King County but is not used directly
in the price models.

---

### 2.3 Hypothesis Tests

I tested how house prices relate to the livability indicators using correlation
and two-sample t-tests.

#### H1: Public school density vs house prices

- *Null hypothesis (H0):* Public school count in a ZIP code is not associated with
  house prices.
- *Alternative hypothesis (H1):* Public school count is associated with house prices.

*Results*

- Pearson correlation between public_school_count and price:
  - r ≈ -0.26, p < 0.001
- When ZIP codes are split into high vs low public school density (above vs
  below the median):
  - two-sample t-test on prices: t ≈ -35, p < 0.001

*Interpretation*

There is a statistically significant *negative* relationship: ZIP codes with
more public schools tend to have *lower average house prices*.  
This may reflect that dense, older, or more central areas host more public
schools but have smaller or less expensive housing stock.

---

#### H2: Private school density vs house prices

- *H0:* Private school count in a ZIP code is not associated with house prices.  
- *H1:* ZIP codes with more private schools have different (higher) prices.

*Results*

- Pearson correlation between private_school_count and price:
  - r ≈ 0.20, p < 0.001
- High vs low private-school ZIPs:
  - t ≈ 27, p < 0.001

*Interpretation*

There is a significant *positive* relationship: ZIP codes with more private
schools tend to have *higher house prices*, which is consistent with the idea
that private schools cluster in wealthier neighborhoods.

---

#### H3: Hospital availability vs house prices

- *H0:* Hospital count in a ZIP code is not associated with house prices.  
- *H1:* ZIP codes with more hospitals have different house prices.

*Results*

- Pearson correlation between hospital_count and price:
  - r ≈ 0.04, p < 0.001
- High vs low hospital-count ZIPs:
  - t ≈ 8.8, p < 0.001

*Interpretation*

The statistical tests detect a positive association, but the effect size is
very small (r ≈ 0.04), meaning that hospital availability is **not a strong
practical driver** of house prices compared to structural features and school
density.

---

### 2.4 Summary of Phase 2

In this phase I:

1. *Collected and filtered* all datasets to the King County level.  
2. *Aggregated* school and hospital counts per ZIP and merged them with the
   housing dataset to build livability indicators.  
3. Performed *EDA* on house prices and livability variables using histograms,
   scatter plots, and correlation matrices.  
4. Ran *hypothesis tests* (Pearson correlations and two-sample t-tests)
   to quantify the relationships between house prices and:
   - public school density  
   - private school density  
   - hospital density  
5. Conducted a *separate EDA* of incidents by city and year to provide a
   safety context for King County, with a clear note that this dataset cannot
   be merged at ZIP level due to missing geographic identifiers.

Overall, the results suggest that **school-related indicators (especially private
schools) show clearer relationships with house prices** than hospital counts, and
that urban density and educational amenities are important components of
“livability” in King County.

## Phase 3 – Regression Modeling and Interpretation

In Phase 3, the analysis moves beyond exploratory data analysis and hypothesis testing to examine how livability indicators explain house prices when controlling for structural housing characteristics. Multiple linear regression models are used to quantify the joint effects of housing features and livability-related variables.

> *Note:* The notebook 01_eda.ipynb has been updated to include all Phase 3 analyses, models, and interpretations described below.

---

### 3.1 Baseline Model: Structural Housing Features

As a first step, a baseline multiple linear regression model was estimated using only structural housing features:

- sqft_living
- bedrooms
- bathrooms
- grade

The dependent variable is house price.

*Findings:*
- The baseline model achieves an R² of approximately *0.54*, indicating that structural characteristics alone explain about 54% of the variation in house prices.
- sqft_living and grade emerge as the strongest predictors, both with large positive and statistically significant coefficients.
- The negative coefficients for bedrooms and bathrooms reflect that, after controlling for size and quality, additional rooms may correspond to smaller room sizes or different layouts.

This baseline model provides a reference point for evaluating the added value of livability indicators.

---

### 3.2 Full Model: Structural Features + Livability Indicators

The full model extends the baseline specification by adding ZIP-level livability indicators:

- public_school_count
- private_school_count
- hospital_count

*Findings:*
- The full model achieves an R² of approximately *0.62*, representing a meaningful improvement over the baseline model.
- This increase in explanatory power suggests that livability indicators contribute additional information beyond structural housing features.

*Key coefficient interpretations:*
- *Public school count:* Negatively associated with house prices, likely reflecting higher-density or older urban areas rather than school quality itself.
- *Private school count:* Positively associated with house prices, consistent with private schools clustering in wealthier neighborhoods.
- *Hospital count:* Positively associated with house prices, though its effect size is smaller relative to school-related indicators and structural features.

---

### 3.3 Model Comparison

Comparing the baseline and full models highlights the role of livability factors:

- *Baseline R²:* ~0.54  
- *Full Model R²:* ~0.62  

The improvement of roughly *8 percentage points* indicates that livability indicators provide meaningful additional explanatory power when modeling house prices.

---

### 3.4 Log-Price Regression Model

Given the strong right skew in house prices, an additional regression model was estimated using the logarithm of price as the dependent variable.

*Findings:*
- The log-price model achieves an R² of approximately *0.68*, outperforming both raw-price models.
- Coefficients can be interpreted as approximate percentage changes, improving interpretability.

*Key insights:*
- Structural variables (sqft_living, grade) remain dominant predictors.
- Livability indicators retain their signs and statistical significance:
  - Public school count is associated with lower prices.
  - Private school and hospital counts are associated with higher prices.
- These results are consistent with findings from Phase 2 hypothesis tests, strengthening confidence in the conclusions.

---

### 3.5 Limitations

This analysis is limited to King County and may not generalize to other regions. Gun violence incident data could not be incorporated into regression models due to the absence of ZIP-level identifiers and was therefore treated as contextual information only. Additionally, omitted variables such as income levels, zoning regulations, and neighborhood amenities may influence house prices and are not captured in the current models.

---

### Phase 3 Summary

Overall, Phase 3 demonstrates that while structural housing characteristics remain the primary drivers of house prices, livability indicators—particularly school-related measures—provide meaningful and interpretable additional explanatory power, especially when prices are modeled on a logarithmic scale.

## Project Summary

This project investigates the relationship between housing prices and livability indicators in the United States, with a focused empirical analysis on King County, Washington. The central goal was to examine whether commonly cited livability factors—such as education, healthcare access, and safety—help explain variations in home prices beyond basic structural housing characteristics.

The project began with the collection and integration of multiple publicly available datasets, including housing transactions, public and private school locations, hospital locations, and gun violence incidents. To ensure geographic consistency and data quality, the analysis was restricted to King County, where reliable ZIP-level alignment was possible for housing, school, and hospital data. Gun violence data, which lacked ZIP-level identifiers, was analyzed separately to provide contextual safety insights rather than being directly merged into the pricing models.

Overall, the results suggest that while physical housing characteristics remain the dominant determinants of home prices, livability indicators—particularly education-related measures—play a significant and interpretable role in shaping housing values. The project highlights the importance of considering both structural and contextual factors when analyzing housing markets and demonstrates how data integration and multivariate analysis can yield deeper insights into regional price differences.
