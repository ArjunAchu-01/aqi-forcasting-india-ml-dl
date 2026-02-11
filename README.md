# AQI Forecasting for Indian Cities (ML + Deep Learning)

This project builds an end-to-end time-series forecasting pipeline to predict **next-day Air Quality Index (AQI)** for selected Indian cities using pollutant concentration data (PM2.5, PM10, NO2, SO2, CO, O3, NH3).  
AQI is **not provided** in the raw dataset and is computed using **CPCB AQI breakpoints**.

## Cities
- Delhi
- Mumbai
- Bengaluru


## Project Goals
- Compute AQI from pollutant measurements using CPCB methodology
- Perform time-series EDA and robust preprocessing
- Train and compare:
  - **Baselines:** Naïve, Seasonal Naïve
  - **Classical ML:** Linear Regression, Random Forest
  - **Deep Learning:** LSTM, GRU
- Evaluate using **MAE, RMSE, sMAPE**
- Provide reproducible, GitHub-ready outputs (results tables, predictions, plots)

---

## Repository Structure
aqi-forecasting-india-ml-dl/
│
├── data/
│ ├── raw/
│ └── processed/
│
├── notebooks/
│ ├── eda.ipynb
│ ├── preprocessing.ipynb
│ ├── baselines.ipynb
│ ├── ml_models.ipynb
│ ├── dl_lstm_gru.ipynb
│ └── final_comparison.ipynb
│
├── outputs/
│ ├── baseline_results.csv
│ ├── ml_results.csv
│ ├── dl_results_lstm_gru.csv
│ ├── final_model_comparison.csv
│ ├── best_model_by_city.csv
│ ├── ml_test_predictions.csv
│ └── dl_test_predictions_lstm_gru.csv
│
├── reports/
│ └── figures/
│
├── requirements.txt
└── README.md


---

## Data & AQI Computation
The raw dataset contains daily pollutant concentrations. AQI is computed by:
1. Calculating pollutant **sub-indices** using CPCB breakpoint ranges
2. Selecting the **maximum sub-index** as the daily AQI
3. Capping AQI to **[0, 500]**

---

## Train/Validation/Test Split (Time-Series Safe)
Chronological splits were used to avoid leakage:

- **Train:** up to 2023-12-31  
- **Validation:** 2024-01-01 to 2024-06-30  
- **Test:** 2024-07-01 to 2024-12-31  

---

## Models
### Baselines
- **Naïve (Persistence):** AQI(t+1) = AQI(t)
- **Seasonal Naïve:** AQI(t+1) = AQI(t-7)

### Classical ML
- Linear Regression (lag features)
- Random Forest (lag features)

### Deep Learning
- LSTM (14-day lookback sequences)
- GRU (14-day lookback sequences)

---

## Evaluation Metrics
- MAE
- RMSE
- sMAPE

---

## Final Results (Test Set)
Final model selection is based on **lowest test MAE**.

| City | Best Model (Test) | MAE | RMSE | sMAPE |
|------|-------------------|-----|------|-------|
| Bengaluru | Naïve | 23.47 | 30.79 | 28.43 |
| Delhi | Naïve | 29.66 | 41.43 | 17.24 |
| Mumbai | Naïve | 11.18 | 16.19 | 15.70 |

**Key takeaway:** For daily AQI forecasting, the persistence baseline is extremely strong and was difficult to beat consistently across cities. More complex ML/DL models did not provide reliable improvements on the held-out test period.

---

## Visual Results
Plots are generated in:
`reports/figures/`

Example plots include:
- Model comparison (MAE/RMSE/sMAPE) per city on the test set
- Actual vs predicted (test window) for selected models

## Test-set Model Comparison

### Bengaluru
![Bengaluru test MAE](reports/figures/Bengaluru_test_MAE.png)
![Bengaluru test RMSE](reports/figures/Bengaluru_test_RMSE.png)
![Bengaluru test sMAPE](reports/figures/Bengaluru_test_sMAPE.png)

### Delhi
![Delhi test MAE](reports/figures/Delhi_test_MAE.png)
![Delhi test RMSE](reports/figures/Delhi_test_RMSE.png)
![Delhi test sMAPE](reports/figures/Delhi_test_sMAPE.png)

### Mumbai
![Mumbai test MAE](reports/figures/Mumbai_test_MAE.png)
![Mumbai test RMSE](reports/figures/Mumbai_test_RMSE.png)
![Mumbai test sMAPE](reports/figures/Mumbai_test_sMAPE.png)


---

## How to Run
1. Create and activate a virtual environment  
2. Install requirements:
```bash
pip install -r requirements.txt



Limitations & Future Work

Daily AQI is highly autocorrelated → persistence baselines are very strong

Adding meteorological variables (wind, humidity, temperature) may improve forecasting

Hourly AQI forecasting may reveal stronger temporal patterns for DL models

More advanced DL (attention/transformers) could be explored with larger datasets