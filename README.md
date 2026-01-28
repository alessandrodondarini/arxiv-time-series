# Time Series Forecasting of Daily Publication Volume Using Statistical and Machine Learning Models

arXiv is a free, open-access repository for scientific publications spanning fields such as Physics, Mathematics, Computer Science & AI, Statistics, and more. New papers are posted daily (excluding weekends), each identified by a unique ID and assigned subject-area labels (“categories”) that help readers navigate research by field.

In this project, we analyze **over 1 million papers published between 2007 and 2025**, using data from the [arXiv Dataset](https://www.kaggle.com/datasets/jimmyyang0928/arxiv-dataset). The goal is to monitor daily publication counts, uncover trends in scientific output, and identify the growth of specific research areas.

Forecasting publication trends is valuable for research planning and practical decision-making. Understanding which fields are growing can inform **funding allocation**, reveal emerging topics, and provide insight into **overall research dynamics**.

From a technical standpoint, forecasting daily publication counts is a **challenging time-series problem**. This project explores three approaches:  
1. **Feature-based machine learning (XGBoost)** – the primary forecasting model  
2. **Statistical modeling (SARIMA)** – an interpretable alternative  
3. **Naive seasonal forecast** – a baseline for reference

---

## Tech Stack
- **Python:** NumPy, Pandas, Matplotlib, Scikit-learn, XGBoost, SciPy, statsmodels  
- **Jupyter Notebook**

---

## Project Overview

The target is the **total number of daily publications**. After cleaning and aggregating the data, the dataset links **dates** to the **number of publications per day**.  

The overall trend is illustrated in Figure 1.

![Daily Paper Evolution](results/arXiv_TimeSeries.png)  
*Figure 1: Time series of daily arXiv publications. The red line represents the 365-day rolling average.*

Figure 2 shows the most popular categories, highlighting that AI-related fields (`cs.CV`, `cs.LG`, `cs.CL`) have overtaken physics-related categories (`hep-ph`, `quant-ph`) in daily publications since ~2018.

![Daily Paper Evolution per Category](results/arXiv_TimeSeries_cat.png)  
*Figure 2: Rolling average (365-day window) of the five most popular primary categories.*

---

## Models Implemented

### 1. XGBoost (ML Model)
XGBoost is a gradient boosting method that models non-linear relationships and complex interactions in the data.  

**Feature engineering applied:**
- **Lagged features:** `lag7`, `lag30`, `lag60`, `lag365`  
- **Rolling features:** `roll7`, `roll90`, `roll180`  

These features allow XGBoost to capture **short-term fluctuations** and **long-term trends**, making it the primary forecasting model for this dataset.

### 2. SARIMA (Alternative Statistical Model)
SARIMA captures temporal dependencies and seasonality after transforming the series to improve stationarity (differencing and seasonal differencing). Model parameters were selected based on **ACF and PACF analysis**, balancing complexity and interpretability.  

**Selected model:**  
\[
SARIMA(13,1,2)(0,1,1)_7
\]

SARIMA provides an **interpretable alternative**, complementing the ML-based approach.

### 3. Naive Seasonal Forecast (Baseline)
A naive seasonal forecast assumes the series repeats its pattern from the previous week. This serves as a **baseline reference**, allowing us to evaluate the added value of the more sophisticated models.

---

## Model Evaluation

![Test comparison](results/XGBoost_predictions.png)


Predictions were evaluated by comparing the forecasted values to the actual observations.  

| Model | MAE (papers/day) | Improvement vs naive baseline |
|-------|-----------------|------------------------------|
| Naive seasonal | 100 | — |
| SARIMA | 89 | 11% |
| XGBoost |88 | 12% |

Both SARIMA and XGBoost outperform the naive seasonal forecast, demonstrating that the models capture **meaningful temporal patterns**. Overall, we see that we managed to obtain a **12% improvement** with respect to the naive baseline.

---

## Conclusions & Outlook

This project demonstrates how **feature-based machine learning** and **statistical models** can be applied to noisy, non-stationary time series. Both XGBoost and SARIMA perform comparably, highlighting the importance of **feature engineering** and **baseline comparison** in practical forecasting tasks.

**Key takeaways & next steps:** 
- Hyperparameter tuning and robust validation strategies could improve model reliability.  
- The workflow demonstrates **decision-oriented modeling**, comparing statistical baselines with ML approaches for practical forecasting.
