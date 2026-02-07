# Detailed Model Methodology Explanation

## 1. Problem Type Identification: Why is this a Regression Problem?

### Difference Between Regression and Classification

**Classification Problem**: The target is **discrete category labels**.
- Example: Predicting whether an email is spam (Yes/No)
- Example: Predicting which product a customer will buy (Category A/B/C)

**Regression Problem**: The target is a **continuous numerical value**.
- Example: Predicting house prices ($100,000 to $1,000,000)
- Example: Predicting temperature (-10°C to 40°C)

### Judgment for This Project

In this project, we aim to predict the target variable **PE (Net Hourly Electrical Energy Output)**:
- **Data Type**: Continuous numerical value
- **Value Range**: 420.26 ~ 495.76 **MW (MegaWatt)**
- **Characteristics**: Can take any value within this range, such as 445.23 MW, 478.91 MW, etc.
- **Unit Explanation**: Since the target variable PE is measured in MW, the prediction error MAE is also in MW

**Conclusion**: Since the target variable PE is a continuous numerical variable rather than discrete categories, this is a **Regression Problem**.

---

## 2. Feature Selection: Why Use These Four Features?

### Selected Features

This project uses all four environmental sensor features:

1. **AT (Ambient Temperature)** - Ambient Temperature (1.81°C ~ 37.11°C)
2. **V (Exhaust Vacuum)** - Exhaust Vacuum (25.36 ~ 81.56 cm Hg)
3. **AP (Ambient Pressure)** - Ambient Pressure (992.89 ~ 1033.30 millibar)
4. **RH (Relative Humidity)** - Relative Humidity (25.56% ~ 100.16%)

### Reasons for Selection

#### 2.1 Clear Physical Meaning
These features are all actual environmental sensor data that directly affect power plant operational efficiency:
- **Ambient Temperature**: Affects cooling system efficiency and thermodynamic cycle
- **Exhaust Vacuum**: Reflects steam turbine efficiency
- **Ambient Pressure**: Affects air density and combustion efficiency
- **Relative Humidity**: Affects cooling system performance

#### 2.2 Correlation with Target Variable
Correlation analysis shows that all features are significantly correlated with PE (power output):
- **AT (Temperature)**: Strongest negative correlation (higher temperature, lower output)
- **V (Vacuum)**: Negative correlation (higher vacuum, lower output)
- **AP (Pressure)**: Positive correlation (higher pressure, higher output)
- **RH (Humidity)**: Negative correlation (higher humidity, lower output)

#### 2.3 Good Data Quality
- No missing values
- Reasonable value ranges
- No obvious outliers

**Conclusion**: All four features have practical significance and are correlated with the target variable, so all are included in the model.

---

## 3. Algorithm Selection: Why Choose These Two Methods?

### 3.1 Linear Regression

#### Characteristics
- **Parametric Algorithm**
- Assumes a **linear relationship** between features and target variable
- Formula: PE = β₀ + β₁×AT + β₂×V + β₃×AP + β₄×RH

#### Reasons for Selection
1. **As Baseline Model**: Simple and easy to interpret, suitable as a comparison baseline
2. **High Computational Efficiency**: Fast training speed, suitable for quick validation
3. **Strong Interpretability**: Each feature's coefficient directly explains its impact on the target
4. **Few Parameters**: Less prone to overfitting

#### Applicable Scenarios
- When the relationship between features and target is relatively linear
- When a quick initial model is needed
- When high model interpretability is required

### 3.2 Random Forest Regressor

#### Characteristics
- **Non-parametric Ensemble Algorithm**
- Composed of multiple decision trees, predicting through voting or averaging
- No need to assume data distribution form

#### Reasons for Selection
1. **Handle Non-linear Relationships**: Automatically captures complex interactions between features
2. **Strong Anti-overfitting Capability**: Reduces overfitting risk through ensemble of multiple trees
3. **Insensitive to Feature Scale**: No need for standardization or normalization
4. **Provides Feature Importance**: Can evaluate which features are most important for prediction

#### Applicable Scenarios
- When the relationship between features and target is complex and non-linear
- When interactions may exist between features
- When data volume is sufficiently large (this project has 9,568 records)

### 3.3 Why Compare These Two Algorithms?

| Characteristic | Linear Regression | Random Forest |
|----------------|-------------------|---------------|
| Model Type | Parametric | Non-parametric |
| Complexity | Simple | Complex |
| Training Speed | Fast | Slower |
| Handle Non-linearity | Weak | Strong |
| Overfitting Risk | Low | Medium (reduced by ensemble) |
| Interpretability | High | Medium |

**Conclusion**: By comparing a **simple parametric model** with a **complex non-parametric model**, we can assess data complexity and choose the most suitable prediction method.

---

## 4. Evaluation Metric: Why Choose MAE?

### Definition of MAE (Mean Absolute Error)

$$\text{MAE} = \frac{1}{n} \sum_{i=1}^{n} |y_i - \hat{y}_i|$$

Where:
- $y_i$ is the actual value
- $\hat{y}_i$ is the predicted value
- $n$ is the number of samples

### Reasons for Choosing MAE

#### 4.1 Intuitive and Easy to Understand
- MAE has the same unit as the target variable (MW)
- Can be directly interpreted: "average prediction error is X MW"
- Easier for non-technical stakeholders to understand

#### 4.2 Robust to Outliers
- MAE uses absolute values, doesn't overly amplify the impact of large errors
- In contrast, MSE/RMSE uses squares, which over-penalize extreme errors

#### 4.3 Reflects Practical Needs
In power plant management, we care about:
- "On average, how much will our predictions deviate from actual values?"
- MAE directly answers this question

### MAE vs Other Metrics

| Metric | Advantages | Disadvantages |
|--------|-----------|---------------|
| **MAE** | Intuitive, robust to outliers | Mathematically non-differentiable at zero |
| **MSE** | Good mathematical properties, differentiable | Unintuitive unit (MW²) |
| **RMSE** | Same unit as target | Sensitive to outliers |
| **R²** | Shows explained proportion | Doesn't reflect actual error magnitude |

**Conclusion**: MAE is the most suitable primary evaluation metric for this project, but we also report R² to illustrate the model's explanatory power.

---

## 5. Validation Strategy: Why Use 5-Fold Cross-Validation?

### 5.1 Data Splitting Strategy

#### Initial Split
- **Training Set**: 80% (~7,654 records)
- **Test Set**: 20% (~1,914 records)

**Purpose**:
- Training set used for model training and comparison
- Test set **completely reserved**, used only for final model evaluation
- Ensures test results are **unbiased and reliable**

#### K-Fold Cross-Validation
- Split training set into **K=5** parts
- Each time, train on 4 parts and validate on 1 part
- Repeat 5 times, ensuring each part is used as validation set once

### 5.2 Why Choose 5-Fold?

#### Advantages
1. **Maximize Training Data Usage**: Each time uses 80% of training set (~6,123 records) for training
2. **More Robust Evaluation**: Averaging 5 validation results reduces randomness of single split
3. **Reasonable Computational Cost**: Compared to 10-Fold, fewer training iterations but still sufficient validation reliability
4. **Suitable for Medium-sized Datasets**: For 7,654 training records, 5-Fold is common and effective

#### Comparison with Other Methods

| Method | Training Data Usage | Reliability | Computational Cost |
|--------|--------------------|--------------|--------------------|
| **Hold-out (70/30)** | 70% | Low (single split) | Low |
| **3-Fold CV** | 66.7% | Medium | Low |
| **5-Fold CV** | 80% | High | Medium |
| **10-Fold CV** | 90% | Very High | High |
| **LOOCV** | 99.9% | Highest | Extremely High |

**Conclusion**: 5-Fold CV achieves the best balance between data utilization, reliability, and computational cost.

---

## 6. How to Avoid Overfitting?

### 6.1 What is Overfitting?

**Overfitting**: The model performs very well on training data but poorly on unseen test data.

### 6.2 Prevention Measures in This Project

#### Measure 1: Reserve Independent Test Set
- Test set **never used for model training or selection**
- Ensures final evaluation results are **unbiased**

#### Measure 2: Use Cross-Validation
- 5-Fold CV evaluates model's **generalization ability** on different data subsets
- Prevents model from just "memorizing" a specific validation set

#### Measure 3: Compare Simple and Complex Models
- Linear Regression: Low complexity, low overfitting risk
- Random Forest: More complex, but reduces overfitting through ensemble

#### Measure 4: Random Forest's Built-in Mechanisms
- **Bootstrap Aggregating (Bagging)**: Each tree trained on different data subset
- **Random Feature Selection**: Each split only considers partial features
- **n_estimators=100**: Ensemble of 100 trees reduces single tree overfitting

### 6.3 How to Determine if Overfitting Occurs?

Observation indicators:
- **Training MAE << Validation MAE**: Possible overfitting
- **Cross-Validation MAE ≈ Test MAE**: Good generalization ability

---

## 7. Model Selection Process Summary

```
Raw Data (9,568 records)
    ↓
Initial Split (80/20)
    ↓
Training Set (80%)         Test Set (20%) ← Completely reserved
    ↓
5-Fold Cross-Validation
    ↓
Compare Linear Regression vs Random Forest
    ↓
Select model with lowest average MAE
    ↓
Retrain on complete training set
    ↓
Calculate final MAE and R² on test set
    ↓
Report final results
```

---

## 8. Conclusion

### Why is This Methodology Sound?

1. **Correct Problem Definition**: Identified as regression problem, chose appropriate algorithms and metrics
2. **Reasonable Feature Selection**: All features have physical meaning and are correlated with target
3. **Algorithm Diversity**: Compared parametric and non-parametric methods, assessed data complexity
4. **Appropriate Evaluation Metric**: MAE is intuitive and meets practical needs
5. **Rigorous Validation Strategy**: Used cross-validation and independent test set to ensure reliable results
6. **Avoid Overfitting**: Multiple mechanisms ensure model's generalization ability

This methodology follows **Machine Learning Best Practices**, ensuring model reliability and practicality.
