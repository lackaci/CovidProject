# CovidProject
This repository contains the code and analysis for a comprehensive study of COVID-19 case trends, government policy interventions, and short-term forecasting across multiple countries from 2020-2022.
# COVID-19 Case Trends, Policy Response & Short-Term Forecasting

This repository contains the code and analysis for a comprehensive study of COVID-19 case trends, government policy interventions, and short-term forecasting across multiple countries from 2020-2022.

## 📋 Overview

This project investigates two main questions:

1. **Descriptively**: How did case/death trends and government interventions (mask mandates, closures, etc.) evolve together across countries?
2. **Predictively**: Can government policy indicators, combined with recent case history, forecast short-term case trends?

Two modeling approaches are compared:
- **PCA + MLP Regressor**: Predicting case counts directly from policy indicators
- **LSTM Classifier**: Predicting whether cases will rise or fall next week

## 📊 Data Sources

- **ECDC COVID-19 Case Data** (2020-2022) - Daily and weekly case/death counts
- **Government Policy Response Measures** - Mask mandates, closures, stay-at-home orders, etc.
- **Population Data** - For per-capita normalization

## 🔍 Key Features

- **Exploratory Data Analysis**: Visualizations of case trends across countries and continents
- **Policy Visualization**: Overlay government measures on case/death curves
- **Feature Engineering**: One-hot encoded policy measures expanded to weekly granularity
- **PCA Analysis**: Country clustering by case load and population
- **Predictive Modeling**:
  - Naive baseline (PCA + MLP) demonstrating why autoregressive features are essential
  - LSTM classifier with sequence modeling for direction prediction
- **Time-based evaluation**: Comparing train/test splits across different pandemic phases

## 📈 Results Summary
In progress.

## 🛠️ Technical Stack

- **Python 3.8+**
- **Libraries**: pandas, numpy, matplotlib, scikit-learn, tensorflow/keras
- **Notebook**: Jupyter (.ipynb)

## 📁 Repository Structure
