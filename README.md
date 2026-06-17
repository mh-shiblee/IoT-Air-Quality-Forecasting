# IoT Air Quality Trend Prediction – CO Forecasting

## Project Overview

This project demonstrates an end-to-end machine learning pipeline for forecasting air pollution levels using real-world IoT sensor data.

The objective is to predict **next-hour Carbon Monoxide (CO) concentration** using historical environmental sensor readings and time-based features.

This work simulates a real-world **industrial IoT environmental monitoring system**, where multiple gas sensors continuously collect atmospheric data for pollution tracking and early warning systems.


##  Target Variable

The model predicts:

> **CO concentration at the next hour (COₜ₊₁)**

The ground-truth CO measurement (`CO(GT)`) represents values recorded by a reference-grade analyzer. The forecasting setup ensures that only past and present information is used to predict future values, mimicking real-world deployment conditions.

---

##  Dataset

- **Dataset Name:** Air Quality Data Set (UCI / Kaggle)
- **Source:** https://www.kaggle.com/datasets/fedesoriano/air-quality-data-set
- **Total Records Used:** 6,937 (after cleaning)
- **Sensor Variables Include:**
  - CO(GT)
  - NOx(GT)
  - NO2(GT)
  - Benzene (C6H6)
  - Temperature (T)
  - Relative Humidity (RH)
  - Absolute Humidity (AH)
  - Multiple PT08 sensor response signals

---

##  Data Cleaning Methodology

During exploratory analysis, the following issues were identified:

### 1️ Hidden Missing Values
The dataset encodes missing sensor readings as -200.  
Since negative gas concentration is physically impossible, these values were:

- Replaced with NaN
- Properly handled before modeling

### High-Missing Column Removal
The column NMHC(GT) contained excessive missing values (>80%) and was removed to preserve dataset integrity.

### Missing Row Handling
Rows containing missing values in the target variable were removed due to their small proportion in the dataset.

###  Time Index Creation
Date and Time columns were combined into a proper datetime index to enable time-series modeling.

---

##  Feature Engineering

To capture temporal dynamics in pollution data, the following features were engineered:

### Lag Features
- CO_lag1
- CO_lag2
- CO_lag3

These capture short-term autocorrelation in pollution levels.

### Rolling Window Feature
- CO_roll3

A 3-hour rolling mean was added to smooth sensor noise and represent short-term trends.

### Time-Based Features
- Hour of day
- Day of week
- Month

These capture cyclical environmental patterns such as traffic peaks and seasonal variations.

---

## Train-Test Split Strategy

To prevent data leakage:

- The dataset was split **chronologically**
- First 80% → Training set  
- Last 20% → Testing set  
- No random shuffling was applied

This ensures realistic forecasting conditions.

---

## Model Training

Five regression models were evaluated:

1. Linear Regression
2. Ridge Regression
3. Random Forest Regressor
4. Gradient Boosting Regressor
5. XGBoost Regressor

This allowed comparison between linear and nonlinear modeling approaches.

---

## Model Evaluation

Models were evaluated using:

- **MAE (Mean Absolute Error)**
- **RMSE (Root Mean Squared Error)**

### Final Results

| Model | MAE | RMSE |
|-------|------|------|
| Gradient Boosting | 0.4144 | 0.5896 |
| Random Forest | 0.4182 | 0.5935 |
| XGBoost | 0.4123 | 0.5980 |
| Ridge | 0.4564 | 0.6675 |
| Linear Regression | 0.4565 | 0.6675 |

✅ **Selected Model:** Gradient Boosting Regressor  
✅ **Final RMSE:** 0.5896  

Given that the mean CO concentration is 2.18 and standard deviation is 1.44, the model effectively captures short-term pollution trends while handling real-world sensor noise.

---

## Visualization

The final model’s predictions were plotted against actual ground-truth values to evaluate trend tracking performance.

Results include:

- Model comparison bar chart
- Actual vs Predicted CO trend
- Zoomed-in forecast visualization

All output plots are stored in the /results directory.

---
## ▶ How to Run

1. Clone the repository
2. Install required libraries:
3. Open the notebook:
 # Assessment 2:Air_quality.ipynb
4. Run all cells sequentially

---

## ✅ Key Takeaways

- Real-world IoT datasets contain hidden missing values and sensor anomalies.
- Temporal feature engineering significantly improves forecasting performance.
- Tree-based ensemble models outperform linear models for nonlinear environmental data.
- Proper chronological splitting is essential to avoid data leakage in time-series forecasting.

---

## 📎 Notebook Link

[Add your notebook link here]

---

## 📎 Dataset Link

https://www.kaggle.com/datasets/fedesoriano/air-quality-data-set

---
