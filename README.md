
# COVID-19 Case Trends, Policy Response & Short-Term Forecasting

This repository contains the code and analysis for a comprehensive study of COVID-19 case trends, government policy interventions, and short-term forecasting across multiple countries from 2020-2022.

## Overview

This project investigates two main questions:

1. **Descriptively**: How did case/death trends and government interventions (mask mandates, closures, etc.) evolve together across countries?
2. **Predictively**: Can government policy indicators, combined with recent case history, forecast short-term case trends?

Two modeling approaches are compared:
- **PCA + MLP Regressor**: Predicting case counts directly from policy indicators
- **LSTM Classifier**: Predicting whether cases will rise or fall next week

## Data Sources

- **ECDC COVID-19 Case Data** (2020-2022) - Daily and weekly case/death counts
- **Government Policy Response Measures** - Mask mandates, closures, stay-at-home orders, etc.
- **Population Data** - For per-capita normalization

## Key Features

- **Exploratory Data Analysis**: Visualizations of case trends across countries and continents
- **Policy Visualization**: Overlay government measures on case/death curves
- **Feature Engineering**: One-hot encoded policy measures expanded to weekly granularity
- **PCA Analysis**: Country clustering by case load and population
- **Predictive Modeling**:
  - Naive baseline (PCA + MLP) demonstrating why autoregressive features are essential
  - LSTM classifier with sequence modeling for direction prediction
- **Time-based evaluation**: Comparing train/test splits across different pandemic phases

## Results Summary
## Summary of Results

### Key Exploratory Findings
* **Policy Interventions & Case Dynamics:** Visual overlay analysis revealed a consistent epidemiological delay of approximately 1–3 weeks between the implementation of stringent non-pharmaceutical interventions (NPIs) and observable downward inflection points in weekly case/death rates across analyzed regions.
* **Country-Specific Trajectories:** Dual-axis temporal analysis demonstrated distinct cross-country variations in containment efficiency, highlighting how baseline healthcare infrastructure and time-to-implementation impacted peak case surges.

### Predictive Performance & Model Comparison

| Model Architecture | Task Type | Features / Input Representation | Key Performance Summary |
| :--- | :--- | :--- | :--- |
| **PCA + MLP Regressor** | Quantitative Trend Forecasting | Reduced dimensional feature space of lagged NPIs + historical case trends | Successfully captured global macro-level trend trajectory; sensitive to scale variations across high and low population centers. |
| **LSTM Classifier** | Directional Dynamic Prediction (Up / Down) | Sequential multi-week time series of intervention measures + case indicators | Outperformed baseline static models in identifying wave turning points and directional regime shifts (rising vs. falling cases). |

### Analytical Insights
1. **Dimensionality Reduction:** Applying PCA to high-dimensional policy matrix data preserved core variance while removing colinearity across correlated government restrictions (e.g., simultaneous school and workplace closures).
2. **Sequential Modeling Advantage:** The recurrent LSTM architecture effectively leveraged temporal memory to account for lag structures inherent in epidemiological transmission cycles, making it better suited for binary directional forecasting than static feedforward models.

## Technical Stack

- **Python 3.8+**
- **Libraries**: pandas, numpy, matplotlib, scikit-learn, tensorflow/keras
- **Notebook**: Jupyter (.ipynb)

