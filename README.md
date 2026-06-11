# NASA Battery Remaining Useful Life (RUL) Prediction

## Project Overview
This project analyses NASA's Li-Ion Battery Dataset from the Prognostics Center of Excellence (PCoE). The goal is to predict the Remaining Useful Life (RUL) of lithium-ion batteries — how many discharge cycles a battery has left before it reaches end-of-life (EOL).

A battery is considered dead when its capacity drops 30% below the rated 2 Ahr capacity, which is **1.4 Ahr**.

---

## Dataset
- **Source**: [NASA Prognostics Data Repository](https://www.nasa.gov/intelligent-systems-division/discovery-and-systems-health/pcoe/pcoe-data-set-repository/)
- **Dataset**: Battery Data Set (Dataset 5)
- **Batteries**: 34 unique Li-Ion batteries
- **Total time-steps**: 835,422 rows
- **Total discharge cycles**: 2,906

Each battery was repeatedly charged and discharged until end-of-life. The data records voltage, current, temperature, and capacity at every time-step of every cycle.

---

## Project Structure

    nasa-battery-rul/
      nasa_battery_rul_analysis.ipynb
      outputs/
        battery_cycle_summary.csv
        battery_cycle_summary_with_rul.csv
        1_capacity_fade.png
        2_actual_vs_predicted_rul.png
        3_model_comparison.png
        4_feature_importance.png

---

## Steps Taken

### 1. Flattening
The raw dataset comes in nested zip files containing MATLAB .mat files. These were extracted and converted into flat CSV tables using Python and scipy.

### 2. Feature Engineering
Two extra features were created:
- **capacity_fade** — how much capacity has been lost from the first cycle
- **rolling_cap_mean** — 5-cycle rolling average of capacity

### 3. RUL Labelling
For each battery, the end-of-life cycle was identified and each discharge cycle was labelled with how many cycles remained until EOL.

### 4. Predictive Modelling
Three models were trained and compared:

| Model | MAE | RMSE | R2 |
|---|---|---|---|
| Linear Regression | 14.79 | 20.21 | 0.49 |
| Gradient Boosting | 2.54 | 4.17 | 0.98 |
| **Random Forest** | **0.68** | **1.66** | **0.9966** |

Random Forest was the best performing model with an R2 of 0.9966 — predicting RUL with 99.66% accuracy.

---

## Key Findings
- Battery capacity degrades gradually and consistently over discharge cycles
- The most important features were discharge cycle number, capacity fade, and rolling capacity mean
- Random Forest significantly outperformed Linear Regression, confirming the non-linear nature of battery degradation

---

## Tools Used
- Python 3
- Jupyter Notebook
- pandas, numpy, scipy
- scikit-learn
- matplotlib

---

## How to Run
1. Download the dataset from the NASA link above
2. Place the zip file in a data/ folder
3. Open nasa_battery_rul_analysis.ipynb in Jupyter Notebook
4. Run all cells in order