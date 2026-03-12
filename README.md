# Lab 5 – Scalable Feature Extraction and Selection for Predictive Maintenance

Name: Aneeha Sohail  
Student ID: 60105845  
Course: DSAI3202 – Programming IoT Applications  
Lab 5 – Scalable Feature Extraction and Selection  
---

# Project Objective

The objective of this lab is to develop a scalable predictive maintenance pipeline using the NASA CMAPSS turbofan engine dataset. The goal is to predict Remaining Useful Life (RUL) using time-series feature extraction, feature selection techniques, and regression models while reducing feature dimensionality without sacrificing performance.

---

# Dataset Description

The CMAPSS dataset contains simulated turbofan engine degradation data. Each engine operates until failure and contains sensor measurements collected over multiple cycles.

Each record contains:

- Unit number (engine ID)
- Time cycles
- 3 operational settings
- 21 sensor measurements

FD001 subset was used because it contains:

- 1 operating condition
- 1 fault mode
- manageable complexity for pipeline development

Training data contains full failure trajectories while test data contains truncated trajectories with provided RUL values.

---

# Methodology

## Step 1 – Data Preprocessing

The following preprocessing steps were performed:

- Removed empty columns
- Assigned column names
- Computed Remaining Useful Life (RUL)

RUL was calculated as:

RUL = max cycle of engine − current cycle

For test data, the provided RUL file was used to compute the true RUL.

---

## Step 2 – Time Window Selection

To standardize time series length, the last 30 cycles of each engine were selected. This captures degradation behaviour close to failure while keeping sequences consistent.

---

## Step 3 – Feature Extraction

Time-series features were extracted using the **tsfresh** library. Extracted features included:

- Mean
- Standard deviation
- Min and max values
- Trend features
- Statistical descriptors

Total extracted features:

**18,792 features**

This demonstrates the high dimensionality of time-series feature extraction.

---

## Step 4 – Feature Selection

### Variance Threshold

Low variance features were removed.

Features reduced from:

18,792 → 3,581

---

### Mutual Information Selection

Top 30 features selected based on relevance to RUL.

Features reduced:

3,581 → 30

---

### Genetic Algorithm Optimization

Genetic Algorithm (DEAP) was used to further optimize feature selection.

Features reduced:

30 → 21

The GA maintained the same RMSE while reducing feature count.

---

## Step 5 – Model Training

A Random Forest Regressor was used because:

- Handles nonlinear data well
- Works well with high-dimensional features
- Robust to noise

Evaluation metric used:

Root Mean Squared Error (RMSE)

---

# Results

| Method | Number of Features | RMSE |
|--------|--------------------|------|
| Raw tsfresh | 18792 | Baseline |
| Variance filtering | 3581 | Improved |
| Mutual Information | 30 | 86.19 |
| Genetic Algorithm | 21 | 86.19 |

The GA successfully reduced feature count without reducing accuracy.

---

# Scalability Discussion

The pipeline demonstrates scalability by significantly reducing dimensionality while maintaining model performance.

Feature reduction:

18,792 → 21 features

Benefits include:

- Faster training time
- Lower memory usage
- Improved deployment feasibility
- Better interpretability

The pipeline can also be applied to FD002, FD003, and FD004 datasets without modification.

---

# Challenges

Some challenges encountered:

- Large number of extracted features
- Runtime of tsfresh extraction
- Feature selection trade-offs
- Computational cost of GA optimization

---

# Conclusion

This lab demonstrated a complete predictive maintenance pipeline including feature extraction, feature filtering, genetic optimization, and regression modeling.

The pipeline successfully reduced features from **18,792 to 21** while maintaining RMSE around **86**.

This shows that aggressive feature reduction can improve efficiency without degrading predictive performance.

---

# Technologies Used

Python libraries used:

- pandas
- numpy
- tsfresh
- scikit-learn
- DEAP

---

# How to Run

1 Install required libraries:
pip install tsfresh
pip install deap
pip install scikit-learn
pip install pandas
pip install numpy


2 Place CMAPSSData folder in project directory

3 Run Jupyter notebook:
Lab5_Predictive_Maintenance.ipynb


4 Execute all cells in order.
---

# Future Improvements

Possible improvements include:

- Testing additional ML models
- Hyperparameter tuning
- Using deep learning models
- Testing other CMAPSS subsets
- Parallel feature extraction

---


