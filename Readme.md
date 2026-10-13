Motor Temperature Monitoring System# Motor Stator Winding Temperature Prediction

A machine learning project that predicts the stator winding temperature
of a Permanent Magnet Synchronous Motor (PMSM) using its operating
and electrical parameters.


## Overview

Motor winding temperature is an important indicator of a motor's thermal
condition. Excessive temperatures can accelerate insulation degradation
and affect motor reliability.

The objective of this project is to develop and evaluate regression models
that estimate stator winding temperature from motor operating conditions.

The project covers data exploration, feature selection, regression model
training, and performance evaluation.

## Dataset

**Dataset:** Electric Motor Temperature

**Source:** [Kaggle - Electric Motor Temperature](https://www.kaggle.com/wkirgsn/electric-motor-temperature)

The dataset contains measurements collected from a Permanent Magnet
Synchronous Motor (PMSM) operating on a test bench.

- Observations: 1,330,816
- Features: 13 columns
- Experimental profiles: 69
- Sampling frequency: 2 Hz

The dataset is organized into multiple measurement profiles, identified
by the `profile_id` column.

Note: This is a publicly available PMSM dataset, not a dataset specific
to GE Power Systems.

### Target Variable

`stator_winding`

The model predicts the continuous temperature of the motor's stator
winding.

### Model Input Features

The following eight operating parameters are used as model inputs:

| Feature | Description |
|---|---|
| `ambient` | Ambient temperature |
| `coolant` | Coolant temperature |
| `motor_speed` | Motor rotational speed |
| `torque` | Motor torque |
| `i_d` | Direct-axis current |
| `i_q` | Quadrature-axis current |
| `u_d` | Direct-axis voltage |
| `u_q` | Quadrature-axis voltage |

The temperature measurements `pm`, `stator_tooth`, and `stator_yoke`
are excluded from the model inputs to focus on predicting winding
temperature from operating conditions.

The `profile_id` column is used for group-based data splitting and
is not supplied to the model as an input feature.

## Methodology

The project follows these steps:

1. **Data Understanding and EDA**
   - Inspect dataset structure and data types.
   - Check missing values and duplicate records.
   - Analyze feature distributions and potential outliers.
   - Examine correlations between variables.
   - Investigate differences between experimental profiles.

2. **Feature Selection**
   - Define the eight operating parameters as input features.
   - Select `stator_winding` as the prediction target.
   - Exclude internal motor temperature measurements and the profile
     identifier from the model inputs.

3. **Train-Test Split**
   - Use `GroupShuffleSplit` with an 80/20 group-level split.
   - Keep complete profiles in either training or testing.
   - This prevents observations from the same profile from appearing
     in both sets.

4. **Regression Model Development**
   - Dummy Regressor: establishes a simple reference baseline.
   - Linear Regression: provides an interpretable baseline model.
   - HistGradientBoostingRegressor: evaluates whether a nonlinear
     regression approach improves predictions.

5. **Model Evaluation**
   - Mean Absolute Error (MAE)
   - Root Mean Squared Error (RMSE)
   - Coefficient of Determination (R²)

All models are evaluated using the same held-out test observations.

## Models

### 1. Dummy Regressor

Predicts the mean winding temperature observed in the training data,
providing a reference for evaluating whether the input features improve
prediction performance.

### 2. Linear Regression

Learns a linear relationship between the selected operating parameters
and winding temperature.

It serves as the first machine learning baseline.

### 3. HistGradientBoostingRegressor

Uses histogram-based gradient-boosted decision trees to learn nonlinear
relationships between motor operating parameters and winding temperature.

It is compared against Linear Regression to assess whether the more
flexible model improves predictive performance.

## Evaluation Metrics

**MAE — Mean Absolute Error**

Measures the average absolute difference between predicted and actual
temperatures. Lower values indicate smaller average errors.

**RMSE — Root Mean Squared Error**

Measures prediction error while penalizing large errors more heavily.
Lower values indicate better performance.

**R² — Coefficient of Determination**

Measures model performance relative to a mean-based reference.
Higher values generally indicate better explanatory performance.

R² is not a percentage accuracy score.

## Results

Three regression models were evaluated for stator winding temperature
prediction: Dummy Regressor, Linear Regression, and
HistGradientBoostingRegressor.

All models were evaluated on the same held-out test observations using
Mean Absolute Error (MAE), Root Mean Squared Error (RMSE), and the
coefficient of determination (R²).

| Model | MAE (°C) | RMSE (°C) | R² |
|---|---:|---:|---:|
| Dummy Regressor | 25.904 | 30.345 | -0.0004 |
| Linear Regression | 11.149 | 15.071 | 0.7532 |
| HistGradientBoostingRegressor | 10.259 | 14.135 | 0.7829 |

### Findings

The Dummy Regressor provides a reference baseline by predicting the
mean winding temperature observed in the training data.

Linear Regression substantially improves on this baseline, indicating
that the selected motor operating parameters provide useful predictive
information.

HistGradientBoostingRegressor achieves the lowest MAE and RMSE and
the highest R² among the three evaluated models.

Compared with Linear Regression, it reduces MAE by approximately 7.98%
and RMSE by approximately 6.21%.

Based on these results, HistGradientBoostingRegressor is selected as
the predictive model for this project, while Linear Regression is
retained as the interpretable baseline.

These results describe performance on held-out profiles from the
dataset and do not establish performance on other motor designs or
industrial systems.
## Getting Started

### Installation

Clone the repository:

```bash
git clone https://github.com/Rk-creations-dev/Motor_Temperature_Prediction.git
```

Navigate to the project directory:

```bash
cd Motor_Temperature_Prediction
```

Install the required libraries:

```bash
pip install -r requirements.txt
```

