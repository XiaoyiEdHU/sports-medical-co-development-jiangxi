# Spatial Co-development of Sports and Medical Facilities in Jiangxi Province, China (2012–2023)

## Repository Structure

```
├── 01_Data_Preparation.ipynb          # Panel construction, lag/lead variables, coordinate extraction
├── 02_Spatial_Autocorrelation.ipynb    # Global Moran's I analysis (Section 4.2)
├── 03_Correlation_Analysis.ipynb      # Pearson correlation, heatmaps (Section 4.2)
├── 04_CF_Estimation.ipynb             # Causal Forest: year-by-year ATE, CATE, multi-lag, lead placebo (Section 4.3)
├── 05_DML_Estimation.ipynb            # Double ML: year-by-year ATE, lag-1, multi-lag, lead placebo (Section 4.3)
├── 06_Variable_Importance.ipynb       # XGBoost + SHAP feature importance (Section 4.4)
├── 07_Robustness_Checks.ipynb         # Permutation placebo, alternative specs, Conley SE (Section 4.6)
├── 08_Figures.ipynb                   # Main result figures: IRF, lead placebo, alt specs, Conley SE
├── README.md
```

## Requirements

- Python 3.12
- Key packages:

```bash
pip install econml doubleml xgboost scikit-learn pandas numpy geopandas matplotlib scipy shap
```

## Analytical Pipeline

|Step|Notebook|Method|Paper Section|
|---|---|---|---|
|Data preparation|01|Panel construction, lag/lead variables|Section 2|
|Spatial autocorrelation|02|Global Moran's I|Section 4.2|
|Correlation analysis|03|Pearson correlation|Section 4.2|
|Causal Forest estimation|04|`econml.CausalForestDML`, year-by-year, `ate_inference()`|Section 4.3|
|Double ML estimation|05|`doubleml.DoubleMLPLR`, XGBoost learners, 5-fold cross-fitting|Section 4.3|
|Variable importance|06|XGBoost + SHAP TreeExplainer|Section 4.4|
|Robustness checks|07|Permutation placebo (200x), 5 alternative specs, Conley SE|Section 4.6|
|Figures|08|All main result visualizations|Sections 4.3, 4.6|

## Key Methods

- **Double Machine Learning (DML)**: Implemented via `DoubleML` with XGBoost learners and 5-fold cross-fitting. Models trained separately for each year (2012-2023).
- **Causal Forest (CF)**: Implemented via `econml.dml.CausalForestDML` with year-by-year fitting. Statistical inference conducted using `ate_inference()` for asymptotically valid standard errors.
- **Falsification tests**: Lead placebo (D_t+1 -> Y_t), multi-lag analysis (lag 0-5), permutation placebo (200 iterations).
- **Robustness**: Five alternative covariate specifications (baseline, -GDP, +city FE, +lon/lat splines, +FE+splines); Conley spatial HAC standard errors (Bartlett kernel, 10 km cutoff).

## Data Availability

- **POI data**: Obtained from Amap (Gaode) Open Platform under commercial license. Not redistributable. Researchers may apply at https://lbs.amap.com/
- **Covariates**: Publicly available from original sources (see manuscript Section 2):
    - LandScan population data: https://landscan.ornl.gov/
    - WorldPop age-structured data: https://www.worldpop.org/
    - Nighttime light data: Chen et al. (2021)
    - PM2.5 data: Wei et al. (2021)
    - GDP data: Kummu et al. (2018)
    - Road network: https://www.openstreetmap.org/
- **Processed data**: Available from the corresponding author upon reasonable request.
