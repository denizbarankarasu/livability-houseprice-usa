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
