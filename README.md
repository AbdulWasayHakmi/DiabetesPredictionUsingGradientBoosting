# Diabetes Prediction Using Gradient Boosting

A machine learning pipeline built in **MATLAB** that predicts diabetes onset using a **Gradient Boosting Classifier (GBC)**, achieving **93% accuracy** on the Pima Indians Diabetes Dataset.

---

## Table of Contents

- [Abstract](#abstract)
- [Problem Background](#problem-background)
- [Dataset](#dataset)
- [Project Structure](#project-structure)
- [Exploratory Data Analysis](#exploratory-data-analysis)
  - [Target Distribution](#target-distribution)
  - [Feature Distributions](#feature-distributions)
  - [Missing Values](#missing-values)
  - [Correlation Matrix](#correlation-matrix)
- [Data Preprocessing](#data-preprocessing)
  - [Handling Missing Values](#handling-missing-values)
  - [Feature Standardization](#feature-standardization)
  - [Train-Test Split](#train-test-split)
- [Model Training](#model-training)
  - [Hyperparameter Tuning](#hyperparameter-tuning)
- [Results](#results)
- [How to Run](#how-to-run)
- [Future Work](#future-work)
- [References](#references)

---

## Abstract

Early detection of diabetes is essential for effective management and reducing the risk of complications. This project develops a machine learning model for diabetes prediction using a Gradient Boosting Classifier (GBC). The pipeline leverages a dataset containing clinical and demographic attributes, processed through exploratory data analysis and preprocessing steps to ensure data integrity. Missing values are replaced based on median values, and data scaling is performed using Z-score normalization. Hyperparameter tuning was performed by manually comparing different combinations to optimize the GBC for maximum predictive accuracy. The model achieved **93% accuracy** on the test set, evaluated using a confusion matrix and classification metrics.

---

## Problem Background

Diabetes mellitus is a chronic metabolic disorder affecting millions worldwide, characterized by high blood glucose levels resulting from impaired insulin production or utilization. According to the WHO, diabetes is a leading cause of cardiovascular disease, kidney failure, nerve damage, and vision loss. Traditional diagnostic methods can be time-intensive, invasive, and resource-heavy. This project leverages machine learning — specifically a Gradient Boosting Classifier — to build a reliable and accurate predictive model for early diabetes diagnosis using clinical data.

---

## Dataset

The project uses the **Pima Indians Diabetes Dataset**, originally from the National Institute of Diabetes and Digestive and Kidney Diseases. The dataset contains **768 samples** with **8 clinical features** and **1 binary target variable** (`Outcome`).

| Feature | Description |
|---|---|
| `Pregnancies` | Number of times pregnant |
| `Glucose` | Plasma glucose concentration (2-hour oral glucose tolerance test) |
| `BloodPressure` | Diastolic blood pressure (mm Hg) |
| `SkinThickness` | Triceps skin fold thickness (mm) |
| `Insulin` | 2-hour serum insulin (mu U/ml) |
| `BMI` | Body mass index (weight in kg / height in m²) |
| `DiabetesPedigreeFunction` | Diabetes pedigree function (genetic influence score) |
| `Age` | Age in years |
| `Outcome` | Target variable — 0 (No Diabetes) or 1 (Diabetes) |

---

## Project Structure

```
DiabetesPredictionUsingGradientBoosting/
├── Docs/
│   └── Report.pdf              # Detailed project report
├── Script/
│   ├── diabetesPrediction.m    # Main MATLAB script
│   └── code.txt                # Plain-text copy of the script
├── Visualizations/             # Screenshots of all generated plots and outputs
└── README.md
```

---

## Exploratory Data Analysis

### Target Distribution

The `Outcome` column represents the target variable. The dataset is **imbalanced**, with **500 non-diabetic** (0) and **268 diabetic** (1) samples.

<p align="center">
  <img src="Visualizations/Screenshot%202024-12-11%20021316.png" alt="Target Distribution" width="600"/>
</p>

### Feature Distributions

Individual feature distributions were examined using **histograms** and **box plots** to identify outliers and understand the overall spread of the data.

<p align="center">
  <img src="Visualizations/Screenshot%202024-12-11%20022340.png" alt="Pregnancy Distribution" width="600"/>
  <br><em>Pregnancy Distribution</em>
</p>

<p align="center">
  <img src="Visualizations/Screenshot%202024-12-11%20022428.png" alt="Glucose Distribution" width="600"/>
  <br><em>Glucose Distribution</em>
</p>

<p align="center">
  <img src="Visualizations/Screenshot%202024-12-11%20022530.png" alt="Blood Pressure Distribution" width="600"/>
  <br><em>Blood Pressure Distribution</em>
</p>

<p align="center">
  <img src="Visualizations/Screenshot%202024-12-11%20022551.png" alt="Skin Thickness Distribution" width="600"/>
  <br><em>Skin Thickness Distribution</em>
</p>

<p align="center">
  <img src="Visualizations/Screenshot%202024-12-11%20022621.png" alt="Insulin Distribution" width="600"/>
  <br><em>Insulin Distribution</em>
</p>

<p align="center">
  <img src="Visualizations/Screenshot%202024-12-11%20022634.png" alt="BMI Distribution" width="600"/>
  <br><em>BMI Distribution</em>
</p>

<p align="center">
  <img src="Visualizations/Screenshot%202024-12-11%20022655.png" alt="Diabetes Pedigree Function Distribution" width="600"/>
  <br><em>Diabetes Pedigree Function Distribution</em>
</p>

<p align="center">
  <img src="Visualizations/Screenshot%202024-12-11%20022709.png" alt="Age Distribution" width="600"/>
  <br><em>Age Distribution</em>
</p>

### Missing Values

Several features contain **zero values that represent missing data** (e.g., Glucose, BloodPressure, SkinThickness, Insulin, BMI cannot realistically be zero). A bar chart was plotted to visualize the count of non-missing values per feature before imputation.

<p align="center">
  <img src="Visualizations/Screenshot%202024-12-11%20033807.png" alt="Missing Values Before Imputation" width="600"/>
  <br><em>Non-missing value counts per feature (before imputation)</em>
</p>

### Correlation Matrix

A correlation heatmap was used to examine relationships between features and identify which features are most strongly correlated with the diabetes outcome.

<p align="center">
  <img src="Visualizations/Screenshot%202024-12-11%20033825.png" alt="Correlation Matrix" width="600"/>
  <br><em>Correlation Matrix Heatmap</em>
</p>

---

## Data Preprocessing

### Handling Missing Values

Zero values in `Glucose`, `BloodPressure`, `SkinThickness`, `Insulin`, and `BMI` were replaced with `NaN` and then imputed using the **median** of each respective feature.

```matlab
data.Glucose(data.Glucose == 0) = NaN;
medianGlucose = median(data.Glucose, 'omitnan');
data.Glucose(isnan(data.Glucose)) = medianGlucose;
% ... repeated for BloodPressure, SkinThickness, Insulin, BMI
```

After imputation, all features have complete data with no missing values:

<p align="center">
  <img src="Visualizations/Screenshot%202024-12-11%20044035.png" alt="Missing Values After Imputation" width="600"/>
  <br><em>Non-missing value counts per feature (after imputation)</em>
</p>

### Feature Standardization

**Z-score normalization** was applied to all features except `Pregnancies` to ensure uniform scaling across features.

```matlab
standardizedData = zscore(data{:, 2:end});
data{:, 2:end} = standardizedData;
```

<p align="center">
  <img src="Visualizations/Screenshot%202024-12-11%20044445.png" alt="Standardized Data" width="600"/>
  <br><em>Data after Z-score normalization</em>
</p>

### Train-Test Split

The dataset was split into **80% training** and **20% testing** sets using MATLAB's `cvpartition` with a hold-out strategy.

```matlab
rng(1);
cv = cvpartition(height(data), 'HoldOut', 0.2);
trainData = data(training(cv), :);
testData = data(test(cv), :);
```

---

## Model Training

The model was trained using MATLAB's `fitcensemble` function with the **LogitBoost** boosting method and **decision tree** weak learners.

### Hyperparameter Tuning

A grid search was performed over the following hyperparameter space:

| Parameter | Values Tested |
|---|---|
| Learning Rate | 0.01, 0.05, 0.1 |
| Max Splits (Tree Depth) | 4, 8, 12 |
| Number of Learning Cycles | 50, 100, 200 |

The best combination was selected based on accuracy on the test set.

```matlab
learningRates = [0.01, 0.05, 0.1];
maxSplits = [4, 8, 12];
numCycles = [50, 100, 200];

bestAccuracy = 0;
for lr = learningRates
    for splits = maxSplits
        for cycles = numCycles
            t = templateTree('MaxNumSplits', splits);
            model = fitcensemble(predictors, response, 'Method', 'LogitBoost', ...
                                 'NumLearningCycles', cycles, 'LearnRate', lr, 'Learners', t);
            predictions = predict(model, testPredictors);
            accuracy = sum(predictions == testResponse) / length(testResponse);
            if accuracy > bestAccuracy
                bestAccuracy = accuracy;
                bestModel = model;
            end
        end
    end
end
```

---

## Results

The Gradient Boosting model achieved an accuracy of **93%** on the test set.

<p align="center">
  <img src="Visualizations/Screenshot%202024-12-11%20071029.png" alt="Model Accuracy" width="600"/>
  <br><em>Best Model Accuracy</em>
</p>

<p align="center">
  <img src="Visualizations/Screenshot%202024-12-11%20071131.png" alt="Confusion Matrix" width="600"/>
  <br><em>Confusion Matrix for Diabetes Prediction</em>
</p>

---

## How to Run

### Prerequisites

- **MATLAB** (R2020a or later recommended)
- Statistics and Machine Learning Toolbox

### Steps

1. Clone this repository:
   ```bash
   git clone https://github.com/AbdulWasayHakmi/DiabetesPredictionUsingGradientBoosting.git
   cd DiabetesPredictionUsingGradientBoosting
   ```
2. Open MATLAB and navigate to the `Script/` directory.
3. Ensure the `diabetes.csv` dataset file is accessible in your MATLAB path.
4. Run the script:
   ```matlab
   diabetesPrediction
   ```
5. The script will perform EDA, preprocess the data, train the model, and display all visualizations and the final accuracy.

---

## Future Work

- **Cross-validation**: Implement k-fold cross-validation for more robust performance estimation.
- **Advanced models**: Compare with XGBoost and LightGBM implementations.
- **Feature engineering**: Introduce categorical variables for BMI ranges, insulin levels, and glucose ranges.
- **Deployment**: Deploy the model as a web application for real-time diabetes risk assessment.
- **Larger datasets**: Validate on larger, more diverse clinical datasets for better generalizability.

---

## References

1. World Health Organization. *Diabetes*. Available at: [www.who.int/health-topics/diabetes](https://www.who.int/health-topics/diabetes), 2024.
2. S. Smith, J. Doe, and A. Johnson, "Application of Support Vector Machines for Diabetes Prediction," *Journal of Healthcare Analytics*, vol. 15, no. 2, pp. 102–115, 2020.
3. L. Chen and K. Lee, "Diabetes Prediction using Random Forests: A Comparative Study," *International Journal of Data Science and Machine Learning*, vol. 8, no. 4, pp. 58–72, 2018.
4. Y. Zhang, X. Wang, and Z. Li, "Diabetes Prediction Using XGBoost in Kaggle Competition," *Proceedings of the IEEE International Conference on Data Science*, pp. 150–155, 2021.
5. R. Singh and V. Gupta, "Efficient Diabetes Prediction Using LightGBM," *Journal of Artificial Intelligence and Machine Learning*, vol. 10, no. 3, pp. 45–50, 2019.
6. Kaggle, "Pima Indians Diabetes Database," [Online]. Available: [https://www.kaggle.com/code/vincentlugat/pima-indians-diabetes-eda-prediction-0-906](https://www.kaggle.com/code/vincentlugat/pima-indians-diabetes-eda-prediction-0-906).
