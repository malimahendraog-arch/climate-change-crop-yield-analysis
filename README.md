# Climate Change and Crop Yield Analysis in India

A Data Science project exploring the relationship between temperature, rainfall and the yield of four major crops in India: Maize, Rice, Soybean and Wheat.

## Project overview

This project analyses yearly crop-yield data from 2000 to 2024 and examines how it is associated with temperature and rainfall. It uses Python for data cleaning, exploratory analysis, visualisation and Machine Learning.

**Important:** The climate observations were collected from one central-India coordinate and are used as a location-based proxy. They are not an area-weighted or nationwide average of Indian weather.

## Objectives

- Study crop-yield trends for Maize, Rice, Soybean and Wheat.
- Examine the linear association between crop yield and temperature/rainfall.
- Visualise climate variables against crop yield.
- Train separate regression models for the four crops and test them on later years.
- Compare actual and predicted crop yield.

## Data sources

- **Crop yield:** FAOSTAT, India, selected crops, 2000–2024.
- **Climate:** NASA POWER, daily temperature (`T2M`) and corrected precipitation (`PRECTOTCORR`), aggregated by year for 2000–2024 at a central-India coordinate.

The final merged dataset contains 100 crop-year rows (25 years × 4 crops), with 0 missing values and 0 duplicate rows in the reported verification.

## Tools and libraries

- Python
- Google Colab
- Pandas and NumPy
- Matplotlib
- Scikit-learn

## Methodology

1. Download and filter the FAOSTAT crop-yield data for India and the four selected crops.
2. Retrieve daily temperature and precipitation from NASA POWER and aggregate the readings by year.
3. Merge crop and climate data on `Year`.
4. Check missing values and duplicate rows.
5. Calculate Pearson correlations between crop yield and each climate variable for every crop.
6. Create scatter plots for temperature vs yield and rainfall vs yield.
7. Train one `LinearRegression` model per crop using `Year`, `Avg_Temperature` and `Total_Rainfall` as input features.
8. Train on 2000–2019 and evaluate on 2020–2024.
9. Compare actual and predicted yields and calculate MAE, RMSE and R².

## Correlation results

| Crop | Temperature correlation | Rainfall correlation |
|---|---:|---:|
| Maize (corn) | -0.399 | 0.505 |
| Rice | -0.462 | 0.522 |
| Soya beans (Soybean) | -0.038 | 0.314 |
| Wheat | -0.352 | 0.382 |

These are associations in this dataset; they do not establish cause and effect.

## Machine Learning results

**Evaluation period:** 2020–2024 (5 observations per crop; 20 observations total).

| Metric | Overall result |
|---|---:|
| MAE | 124.56 kg/ha |
| RMSE | 151.12 kg/ha |
| R² | 0.984 |

### Crop-wise error

| Crop | MAE (kg/ha) | RMSE (kg/ha) |
|---|---:|---:|
| Maize (corn) | 135.27 | 161.11 |
| Rice | 54.06 | 93.94 |
| Soya beans (Soybean) | 119.66 | 132.92 |
| Wheat | 189.26 | 197.24 |

The overall R² is pooled across the 20 test rows and may be influenced by differences in yield levels between crops. The test set is small, so the metrics should be treated as preliminary rather than proof of strong generalisation.

## Repository structure

Upload the following project files (use these names where possible):

```text
climate-change-crop-yield-analysis/
├── README.md
├── Climate_Change_Crop_Yield_Analysis.ipynb
├── data/
│   ├── Final_Climate_Crop_Dataset.csv
│   ├── Final_Crop_Predictions.csv
│   ├── Crop_Correlation_Results.csv
│   └── Crop_Model_Performance.csv
├── graphs/
│   ├── Crop_Yield_Trends_2000_2024.png
│   ├── Climate_Correlation_Graphs.png
│   └── Final_Prediction_Graph.png
└── report/
    └── Climate_Crop_Yield_Project_Report.pdf
```

Only include the notebook, processed/derived CSV files, final graphs and final report. Do not upload the original multi-million-row FAOSTAT ZIP unless there is a specific reason; it is not needed to view the completed analysis.

## Limitations

- Climate data represents one coordinate, not the whole of India.
- Crop yields are national-level values, so spatial matching between weather and crop yield is limited.
- Only four crops and 25 years are included.
- The models do not include other important factors such as irrigation, soil, fertiliser use, cultivated area, seeds or farming practices.
- Correlation does not imply causation.
- Only five years per crop are used for testing, so performance estimates are based on a small test sample.

## Future improvements

- Use region/state-level crop-yield and climate data with matching spatial coverage.
- Add soil, irrigation, fertiliser, cultivated area and extreme-weather variables.
- Compare additional regression and tree-based models using time-aware validation.
- Study total production and food prices to make a more direct assessment of food supply.

## How to run

1. Open the notebook in Google Colab.
2. Run the cells in order to download, clean, merge, analyse and model the data.
3. Confirm that the data sources and API parameters are still accessible before rerunning the notebook.

## References

- FAOSTAT: https://www.fao.org/faostat/
- NASA POWER: https://power.larc.nasa.gov/
- Pandas: https://pandas.pydata.org/docs/
- Matplotlib: https://matplotlib.org/stable/
- Scikit-learn: https://scikit-learn.org/stable/
