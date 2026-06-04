# 🌧️ Augmenting the Historical Record: Climate Drivers of Sri Lankan Rainfall

> **ST 3011 – Statistical Programming | Group 2**  
> A multivariate statistical analysis of global, regional, and domestic climate drivers of Sri Lankan rainfall (1970–2000)

---

## 👥 Authors

| Student ID | Name |
|---|---|
| s16948 | Akindu Liyanage |
| s16974 | Ayaz Ahamed |
| s16629 | Dilmi Nawodya |
| s16806 | Pasindu Jayasundara |

---

## 📖 Overview

Sri Lanka's hydro-climatology is governed by seasonally varying monsoon systems that directly impact agricultural stability, hydropower generation, and disaster management. This project moves beyond a simple univariate analysis of rainfall by augmenting 30 years of historical station data with global, regional, and local climate indices, enabling a statistically rigorous multivariate investigation.

**Core question:** *How do global, regional, and local climate factors affect rainfall in Sri Lanka?*

---

## 🎯 Objectives

| # | Scale | Focus |
|---|---|---|
| 1 | **Global** | ENSO (Niño 3.4 SST, SOI), Solar Cycles (Sunspot Numbers) |
| 2 | **Regional** | Indian Ocean Dipole (DMI), All-India Rainfall |
| 3 | **Local/Domestic** | 850 hPa Zonal U-Wind circulation |
| 4 | **Spatio-temporal** | Seasonal migration and spatial distribution of rainfall across Sri Lanka |

---

## 📦 Datasets

### Primary Rainfall Data
Historical station data provided by the **Department of Meteorology, Colombo, Sri Lanka** (via Dr. H. K. W. I. Jayawardena, Open University of Sri Lanka).

| Dataset | Period | Stations | Records |
|---|---|---|---|
| Dry Zone Stations | 1970–1999 | 5 (Anuradhapura, Mannar, Puttalam, Trincomalee, Vavuniya) | 52,553 |
| 1990s Stations | 1990–1999 | 10 (Dry + Wet Zone) | 34,283 |

### Augmented Climate Variables

**Global Scale**
- `Nino3.4_SST` — Monthly SST in the Niño 3.4 region (°C) · [HadISST, Met Office](https://www.metoffice.gov.uk/hadobs/hadisst/)
- `Southern_Oscillation_Index` — Normalized Tahiti–Darwin pressure difference · [CRU, UEA](https://crudata.uea.ac.uk/cru/data/soi/)
- `Sunspot_Number` — Daily sunspot count · [WDC-SILSO, Royal Observatory of Belgium](https://sidc.be/SILSO/datafiles)

**Regional Scale**
- `DMI` — Dipole Mode Index, monthly SST gradient in the Indian Ocean (°C) · [NOAA / HadISST1.1](https://psl.noaa.gov/data/timeseries/month/data/dmi.had.long.data)
- `All_India_Monthly_Rainfall_mm` — All-India average monthly rainfall (mm) · [IITM](https://www.tropmet.res.in/data/data-archival/rain/iitm-regionrf.txt)

**Local Scale**
- `U_Wind_850_Mean` — Daily mean zonal wind at 850 hPa (~1.5 km ASL), grid point 6°N 80°E · [NOAA-CIRES-DOE 20CR V3](https://www.psl.noaa.gov/data/gridded/data.20thC_ReanV3.html)

---

## 🔧 Data Preprocessing

- Raw data stored in `.dat` format, extracted via custom Python script
- Missing values flagged as `"M"` (514 values) → imputed using **K-Nearest Neighbors (KNN)**
- Trace rainfall flagged as `"T"` → treated as `0 mm`
- Date decomposed into `Year`, `Month`, `Day` for lagging and seasonal analysis

---

## 📊 Methodology & Key Analyses

### Stationarity & Trend (Objective 1)
- **Mann-Kendall Test** (p = 0.165): No significant monotonic trend
- **Ljung-Box Test** (p = 0.297): Annual series is independently distributed
- **Spline Regression** (p = 0.517): No significant non-linear curvature
- **ADF Test** (p = 0.0014): Series confirmed stationary ✅

### ENSO / Niño 3.4 SST
- Shapiro-Wilk confirmed non-normality with a **positive skew** (more intense El Niño events)
- Mann-Whitney U: El Niño and La Niña phases **not significantly different** from neutral (p > 0.05)

### Southern Oscillation Index
- Pearson Correlation r = 0.038 (p = 0.48): **No significant relationship**
- 12-month lag analysis: No significant lags found

### Sunspot Numbers
- One-Way ANOVA across solar phases: **No immediate effect** (p = 0.316)
- Spearman lagged correlation at **lag = 5 years**: Significant (p = 0.0045) 🌞

### Indian Ocean Dipole (DMI)
- One-sided Mann-Whitney U — Negative DMI → higher rainfall:
  - Dry zone: p = 0.0066 ✅
  - Wet zone: p = 0.0149 ✅

### All-India Rainfall
- Mann-Whitney U — High India rainfall months → higher Sri Lankan rainfall:
  - Both dry and wet zone: p < 0.05 ✅

### U-Wind (Local)
- Clear **bimodal distribution**: Westerlies (SW Monsoon) vs Easterlies (NE Monsoon)
- Mann-Whitney U: Westerlies bring **significantly more rainfall** than Easterlies (p << 0.05) ✅

### Spatio-Temporal Analysis
- **Friedman Test**: Significant seasonality (χ² = 93.94, p < 0.001) and spatial variation (χ² = 52.99, p < 0.001)
- **Rain Shadow Effect** (SW Monsoon): Wet Zone (~8.02 mm/day) receives **4.7×** more rainfall than Dry Zone (~1.70 mm/day)
- **Second Inter-monsoon surge**: Dry Zone jumps to ~8.30 mm/day, exceeding Wet Zone's SW Monsoon average
- **Wind-Rain Correlation Flip**: Positive correlation in SW (Ratnapura r = +0.154) vs negative in NE (Batticaloa r = −0.177)

---

## 📈 Key Findings

1. **Sri Lanka's historical Dry Zone rainfall is stationary** — no long-term trend over 1970–2000
2. **ENSO and SOI have no direct, statistically significant impact** on daily rainfall
3. **Solar activity influences rainfall with a ~5-year lag**, possibly due to ocean thermal inertia
4. **Negative IOD phases significantly increase rainfall** in both wet and dry zones
5. **Indian monsoon rainfall is strongly coupled** with Sri Lankan precipitation
6. **Westerly winds (SW Monsoon) are the dominant local rainfall driver**
7. **Sri Lanka's climate is defined by topographic rain shadow and bimodal monsoon seasonality**, not global indices

---

## 🗂️ Repository Structure

```
├── data/
│   ├── raw/                   # Original .dat station files
│   ├── processed/             # Cleaned & augmented datasets
│   └── external/              # Downloaded climate index CSVs
├── notebooks/
│   ├── 01_preprocessing.ipynb
│   ├── 02_eda.ipynb
│   ├── 03_global_indicators.ipynb
│   ├── 04_regional_indicators.ipynb
│   ├── 05_local_indicators.ipynb
│   └── 06_spatiotemporal.ipynb
├── src/
│   ├── extract_dat.py         # Data extraction script
│   ├── imputation.py          # KNN imputation
│   └── utils.py               # Helper functions
├── figures/                   # Generated plots and visualizations
├── report/
│   └── ST_3011_Group_2_Report.pdf
└── README.md
```

---

## 🛠️ Requirements

```bash
pip install pandas numpy scipy statsmodels matplotlib seaborn scikit-learn pymannkendall
```

---

## 🙏 Acknowledgements

We sincerely thank **Dr. H. K. W. I. Jayawardena** (Senior Lecturer, Department of Physics, Open University of Sri Lanka) for providing the 100-year historical Sri Lanka rainfall dataset and the domain expertise that made this study possible.

---

## 📄 License

This project was produced as academic coursework for ST 3011 – Statistical Programming. Data sources are publicly available or used with permission. Please cite appropriately if referencing this work.
