# 🌾 Climate Change and Crop Yield Analysis in India

**How are temperature and rainfall linked to the yield of Wheat, Rice, Maize and Soybean in India? A 25-year analysis (2000–2024) using Python and Linear Regression.**

![Python](https://img.shields.io/badge/Python-3.x-blue)
![Pandas](https://img.shields.io/badge/Pandas-data%20analysis-150458)
![Scikit-learn](https://img.shields.io/badge/Scikit--learn-ML-orange)
![Matplotlib](https://img.shields.io/badge/Matplotlib-visualisation-11557c)
[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/YOUR-USERNAME/climate-change-crop-yield-analysis/blob/main/Climate_Change_Crop_Yield_Analysis.ipynb)

📄 **Full report (PDF):** [report/Climate_Change_Crop_Yield_India_Report.pdf](report/Climate_Change_Crop_Yield_India_Report.pdf)

---

## 📌 Overview

Climate change can change how well crops grow. In this project I join **crop yield data (FAOSTAT)** with **climate data (NASA POWER)** and study how yearly temperature and rainfall are related to the yield of four major crops in India.

The project covers the full data science workflow: data collection, cleaning, merging, exploration, correlation analysis, machine learning, and honest evaluation of the results.

## ⚡ At a Glance

| | |
|---|---|
| **Crops** | Maize (corn), Rice, Soya beans, Wheat |
| **Country / period** | India, 2000–2024 (25 years) |
| **Dataset size** | 100 rows (25 years × 4 crops), 0 missing values, 0 duplicates |
| **Inputs** | Year, average temperature (°C), total rainfall (mm) |
| **Output** | Crop yield (kg/ha) |
| **Model** | Linear Regression, one model per crop |
| **Train / test split** | Train 2000–2019, test 2020–2024 (time-based) |
| **Overall test MAE / RMSE** | 124.56 / 151.12 kg/ha |
| **Overall test R²** | 0.984 (pooled across 4 crops, see notes below) |

## 🔍 Key Findings

1. **All four crops** show a **negative** correlation with temperature and a **positive** correlation with rainfall.
2. **Rice** has the strongest relationships (temperature −0.462, rainfall 0.522). **Soybean** has almost no link with temperature (−0.038).
3. Yields of **Rice, Wheat and Maize rise clearly over 2000–2024**. Soybean stays low and uneven.
4. **Rice** is predicted most closely (MAE 54.06 kg/ha). **Wheat** has the largest error (MAE 189.26 kg/ha).
5. **A good score is not the same as a climate effect.** In a baseline check, a model using only `Year` predicted the test years about as well as the full model (see [below](#-how-much-does-climate-really-add)). Most of the accuracy comes from the long-term upward trend, not from temperature or rainfall.

> These are **associations in this dataset, not proof of cause and effect.**

## 📊 Visuals

### Crop yield trends (2000–2024)
![Crop yield trends](graphs/Crop_Yield_Trends_2000_2024.png)

### Temperature and rainfall vs yield
![Climate correlation graphs](graphs/Climate_Correlation_Graphs.png)

### Actual vs predicted yield (test years 2020–2024)
![Actual vs predicted yield](graphs/Final_Prediction_Graph.png)

## 🗂️ Data

### Sources

| Data | Source | Details |
|---|---|---|
| Crop yield | [FAOSTAT](https://www.fao.org/faostat/) bulk download (Crops and Livestock Products) | Element = Yield, Area = India, four crops, 2000–2024, unit kg/ha |
| Climate | [NASA POWER](https://power.larc.nasa.gov/) daily point API | `T2M` (temperature at 2 m) and `PRECTOTCORR` (corrected precipitation), 2000–2024 |

> ⚠️ **Important:** The climate data comes from **one central-India point** (latitude 20.5937, longitude 78.9629) and is used as a stand-in for the country. It is **not** a nationwide average of Indian weather.

### How the climate values were made

Daily values were grouped by year: **Avg_Temperature** is the yearly mean of daily temperature, and **Total_Rainfall** is the yearly sum of daily rainfall. This gives 25 yearly rows, which are merged with the crop data on `Year`.

### Main dataset: `data/Final_Climate_Crop_Dataset.csv`

| Column | Meaning |
|---|---|
| `Country` | India |
| `Year` | 2000–2024 |
| `Crop` | Maize (corn), Rice, Soya beans, Wheat |
| `Yield` | Crop yield |
| `Unit` | kg/ha |
| `Avg_Temperature` | Yearly average temperature (°C) |
| `Total_Rainfall` | Yearly total rainfall (mm) |

## 🧭 Methodology

1. Download the FAOSTAT bulk file and keep only India, Element = Yield, and the four crops for 2000–2024.
2. Download daily temperature and rainfall from NASA POWER and group them by year.
3. Merge crop and climate data on `Year`.
4. Check missing values and duplicate rows.
5. Calculate the Pearson correlation between yield and each climate variable, crop by crop.
6. Plot yield trends and scatter plots (temperature vs yield, rainfall vs yield).
7. Train **one `LinearRegression` model per crop** using `Year`, `Avg_Temperature` and `Total_Rainfall`.
8. Train on 2000–2019 and test on 2020–2024, so the models are tested on **future years** they have not seen.
9. Compare actual and predicted yield and calculate MAE, RMSE and R².

## 📈 Results

### Correlation with crop yield

| Crop | Temperature | Rainfall |
|---|---:|---:|
| Maize (corn) | -0.399 | 0.505 |
| Rice | -0.462 | 0.522 |
| Soya beans | -0.038 | 0.314 |
| Wheat | -0.352 | 0.382 |

### Test error by crop (2020–2024)

| Crop | MAE (kg/ha) | RMSE (kg/ha) |
|---|---:|---:|
| Maize (corn) | 135.27 | 161.11 |
| Rice | 54.06 | 93.94 |
| Soya beans | 119.66 | 132.92 |
| Wheat | 189.26 | 197.24 |

> **About the R² of 0.984:** it is calculated on all 20 test rows together. The four crops have very different yield levels (about 1,000 kg/ha for soybean and about 4,000 kg/ha for rice), so a pooled R² can look high even when the within-crop predictions are only fair. The per-crop errors above are a fairer guide. Each crop also has only **5 test points**, so treat the results as preliminary.

### How the model improved

I started with a simple pooled model and improved it step by step. All scores below are pooled over the test rows of that experiment.

| Step | Approach | MAE (kg/ha) | R² |
|---|---|---:|---:|
| 1 | One model for all crops, random 80/20 split | 347.27 | 0.797 |
| 2 | One model, time-based split (test 2020–2024), temperature + rainfall + crop | 570.19 | 0.690 |
| 3 | Same, with `Year` added | 320.24 | 0.897 |
| 4 | **Separate model per crop, with `Year`** (final) | **124.56** | **0.984** |

Step 2 shows that a random split gave an optimistic score. Testing on future years is a harder and more honest test.

## 🧪 How Much Does Climate Really Add?

To check what drives the predictions, I compared three input sets using the same per-crop models and the same train/test years (average MAE across the four crops, in kg/ha):

| Inputs | Average MAE |
|---|---:|
| `Year` only | 112.4 |
| Temperature + rainfall only | 576.4 |
| `Year` + temperature + rainfall (final model) | 124.6 |

**What this means:** `Year` works as a trend variable. It captures things like better seeds, irrigation and farming methods, which push yields up over time. Temperature and rainfall alone predict poorly, and adding them to `Year` does not improve the test error on average. So this project shows **links** between climate and yield, but it does **not** show that climate variables improve yield prediction here. Better regional climate data would be needed to test that properly.

## 📁 Repository Structure

```text
climate-change-crop-yield-analysis/
├── README.md
├── Climate_Change_Crop_Yield_Analysis.ipynb   # Full analysis notebook (with outputs)
├── data/
│   ├── Final_Climate_Crop_Dataset.csv         # Merged crop + climate data (100 rows)
│   ├── Final_Crop_Predictions.csv             # Actual vs predicted, 2020–2024
│   ├── Crop_Correlation_Results.csv           # Correlation by crop
│   └── Crop_Model_Performance.csv             # MAE and RMSE by crop
├── graphs/
│   ├── Crop_Yield_Trends_2000_2024.png
│   ├── Climate_Correlation_Graphs.png
│   └── Final_Prediction_Graph.png
└── report/
    └── Climate_Change_Crop_Yield_India_Report.pdf
```

The raw FAOSTAT download is not stored in the repo because of its size. The notebook downloads it again when you run it.

## ▶️ How to Run

**Google Colab (recommended):**
1. Click the **Open In Colab** badge at the top.
2. Run the cells in order. The notebook downloads the FAOSTAT and NASA POWER data by itself.

**Just want to see the results?** You don't need to run anything. The notebook on GitHub already shows all outputs, and the cleaned data is in the `data/` folder.

## ⚠️ Limitations

- Climate data is from **one location**, not an average of all India.
- Crop yields are **national** values, so weather and yield are not matched by region.
- Only **four crops** and **25 years** are used (100 rows).
- Only **5 test years per crop**, so error estimates are based on very few points.
- `Year` carries most of the predictive power (see the check above).
- No irrigation, soil, fertiliser, cultivated area, seeds or farming-method data.
- Correlation does **not** mean causation.

## 🚀 Future Improvements

- Use **state-level** crop and climate data with matching regions.
- Add irrigation, fertiliser, soil, cultivated area and extreme-weather variables (drought and flood years).
- Detrend the yield data to measure the effect of climate separately from the long-term trend.
- Try more models (Random Forest, XGBoost) with time-aware cross-validation.
- Add total production and food prices to look at food supply more directly.
- Build an interactive dashboard (Streamlit or Power BI).

## 👤 Author

**Mahendra Mali**

- GitHub: (https://github.com/YOUR-USERNAME)
- LinkedIn: (https://www.linkedin.com/in/YOUR-LINKEDIN/)

If this project helped you, feel free to ⭐ the repo.

## 📚 References

- [FAOSTAT](https://www.fao.org/faostat/)
- [NASA POWER](https://power.larc.nasa.gov/)
- [Pandas](https://pandas.pydata.org/docs/) · [Matplotlib](https://matplotlib.org/stable/) · [Scikit-learn](https://scikit-learn.org/stable/)
