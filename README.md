# Predicting Scientific Publication Trends on arXiv: XGBoost vs ARIMA

arXiv is a free, open-access repository for scientific publications spanning fields such as Physics, Mathematics, Computer Science & AI, Statistics, and more. New papers are posted daily (excluding weekends), each identified by a unique ID and assigned subject-area labels (“categories”) that help readers navigate research by field.

In this project, we analyze **1M+ papers published between 2007 and 2025**, using data from the [Kaggle arXiv Dataset](https://www.kaggle.com/datasets/jimmyyang0928/arxiv-dataset). Our goals are to monitor the number of daily publications, uncover long-term trends in scientific output, and identify the growth of specific research categories.

Predicting the evolution of scientific productivity is valuable for both research planning and practical forecasting. Understanding which fields dominate over time can inform the **optimal allocation of funding**, reveal emerging areas of interest, and provide a clearer picture of **overall research dynamics**.

From a technical standpoint, forecasting daily publication counts is a **challenging time-series problem**. This project compares the performance of a **classical statistical model (ARIMA)** and a **modern machine learning model (XGBoost)**, highlighting the strengths and limitations of each approach.



## Project Overview

As a time-series analysis problem, the target we aim to forecast is the **total number of daily publications**. After data cleaning and feature extraction, we obtain a dataset that relates the **date** to the **number of publications on that day**. 

The situation is illustrated in the following plot, which shows the number of daily arXiv publications over time.

![Daily Paper Evolution](results/arXiv_TimeSeries.png)
*Figure 1: Time series of daily arXiv publications. The red line represents the rolling average computed using a 365-day window.*

From Figure 1, it is clear that the number of daily publications has experienced substantial growth in recent years. To better understand this trend, it is particularly interesting to examine the daily publication patterns of the most popular categories.

![Daily Paper Evolution per Category](results/arXiv_TimeSeries_cat.png)
*Figure 2: Rolling average (365-day window) of the five most popular primary categories.  
The blue line corresponds to Computer Science – Computer Vision (`cs.CV`), the orange line to Computer Science – Machine Learning (`cs.LG`), the green line to Computer Science – Computation and Language (`cs.CL`), the red line to Quantum Physics (`quant-ph`), and the purple line to High Energy Physics – Phenomenology (`hep-ph`). For a full description of arXiv categories, see https://arxiv.org/category_taxonomy.*

Figure 2 displays the rolling averages of the top five primary categories, showing that AI-related fields (`cs.CV`, `cs.LG`, `cs.CL`) have substantially overtaken the physics-related categories (`hep-ph`, `quant-ph`), which used to dominate the number of daily publications until around 2018.



## Models Implemented

We have implemented two models to forecast daily arXiv publications:

1. **Machine Learning Model: XGBoost**  
   This algorithm builds multiple decision trees sequentially to iteratively refine predictions by minimizing residual errors. Before implementing the algorithm, we performed feature engineering. In particular, we introduced *lagged features*, which link the current value to specific past values. Specifically, we created the `lag7` column for a 7-day lag (one week), as well as `lag30`, `lag60`, and `lag365` for one-month, two-month, and one-year lags. In addition, adding smoothed features can help the model detect long-term trends. To this end, we introduced rolling averages over 1 week, 3 months, and 6 months, which are stored in the `roll7`, `roll90`, and `roll180` columns.

2. **Traditional Model: ARIMA**  
   A very useful baseline that helped us quantify the gain of using machine learning models in time series analysis. In ARIMA models, the current value at time $t$ is modeled as a linear combination of $p$ lagged values (Autoregressive, AR) and $q$ lagged forecast errors (Moving Average, MA). Further transformations such as a Box-Cox transformation and differencing were implemented to make the time series as stationary as possible.



## Results

![XGBoost Predictions](results/XGBoost_predictions.png)
*Figure 3: Comparison of XGBoost predictions for daily arXiv publications with the actual test set data. The predictions are shown as an orange line, while the actual data are shown as a blue line.*

The results of the XGBoost model are shown in Figure 3, where we overlay the predictions on the actual data for approximately one year. A visual inspection suggests that the model is able to capture the oscillatory behavior of the time series, although it struggles to reproduce most of the resonant peaks.

To assess the predictions quantitatively, we computed the **Mean Absolute Error (MAE)**. XGBoost achieved a MAE of **88.0 papers/day**, while ARIMA had a MAE of **126.8 papers/day**, corresponding to a **31% improvement**. This highlights XGBoost's superior ability to capture non-linear and non-stationary trends in daily publication counts.

---

## Conclusions

This project shows that machine-learning methods such as XGBoost can outperform classical statistical models when dealing with noisy, highly non-stationary time-series data like daily arXiv publications. While ARIMA provides a strong baseline, XGBoost captures long-term trends and nonlinear patterns more effectively, leading to a substantial improvement in predictive accuracy. These results suggest that modern ML approaches offer a promising direction for forecasting scientific output and understanding the evolution of research fields over time.

---
