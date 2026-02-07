# Power Plant Energy Output Prediction Project

[English](#english) | [中文](#中文)

---

## English

**Author**: Lily Wang  
**Project Type**: AI Product Management / Machine Learning  
**Algorithms**: Linear Regression, Random Forest Regressor  

---

### 1. Project Objective

The goal of this project is to predict the net hourly electrical energy output (PE) of a Combined Cycle Power Plant. Accurate predictions are practically important for optimizing plant operations, energy scheduling, and maintenance planning.

The project focuses on four main aspects:

1. Modeling Approach
2. Model Building
3. Model Evaluation
4. Model Interpretation

---

### 2. Modeling Approach

#### 2.1 Problem Type

- **Target Variable**: PE (Net Hourly Electrical Energy Output), measured in MW (MegaWatt)  
- **Value Range**: 420.26 – 495.76 MW  
- **Problem Type**: Regression (predicting continuous numerical values)

📊 **[Visualization Placeholder]**: Dataset preview showing PE column and value ranges  
_Source: `notebooks/power_plant_prediction_en.ipynb`_

#### 2.2 Feature Selection

Four environmental sensor features were used:

| Feature | Description | Range | Correlation with PE |
|---------|------------|-------|------------------|
| AT | Ambient Temperature | 1.81 – 37.11 °C | -0.948 (strong negative) |
| V | Exhaust Vacuum | 25.36 – 81.56 cm Hg | -0.870 (strong negative) |
| AP | Ambient Pressure | 992.89 – 1033.30 millibar | +0.518 (moderate positive) |
| RH | Relative Humidity | 25.56% – 100.16% | +0.390 (weak positive) |

**Rationale for Feature Selection**:

1. Physical significance: Each feature affects power output based on thermodynamics.  
2. Significant correlation with target variable.  
3. High-quality data: 9568 records, no missing or duplicate values.

📊 **[Visualization Placeholder]**: Feature correlation matrix or heatmap  
_Source: `notebooks/power_plant_prediction_en.ipynb`_

#### 2.3 Algorithm Selection

Two algorithms were compared:

1. **Linear Regression**  
   - Parametric, assumes linear relationships  
   - Simple, fast, interpretable  
   - Used as baseline model

2. **Random Forest Regressor**  
   - Non-parametric ensemble algorithm (multiple decision trees)  
   - Captures non-linear relationships and feature interactions  
   - No assumption on data distribution

---

### 3. Model Building

#### 3.1 Data Splitting Strategy

- **Dataset size**: 9568 records  
- **Initial split**: 80% training (~7654), 20% testing (~1914)  
- **Principle**: Test set reserved for final evaluation to prevent bias

📊 **[Visualization Placeholder]**: Data splitting diagram (80/20 + 5-Fold CV illustration)  
_Source: `docs/presentation_script.md` or custom diagram_

#### 3.2 Cross-Validation

- **Method**: 5-Fold Cross-Validation on training set  
- **Purpose**:  
  - Maximize data usage  
  - Obtain robust evaluation  
  - Reduce overfitting risk

#### 3.3 Model Comparison

- **Evaluation Metric**: MAE (Mean Absolute Error)  
- **Scikit-learn note**: `cross_val_score` uses neg_mean_absolute_error to align "higher is better" convention

**5-Fold CV Results**:

| Model | Avg MAE | Std Dev | 5-Fold Range |
|-------|---------|---------|-------------|
| Linear Regression | 3.63 MW | ±0.06 | 3.56–3.71 MW |
| Random Forest | 2.48 MW | ±0.07 | 2.35–2.55 MW |

**Observation**: Random Forest reduces error by 31.68% and shows stable performance.

📊 **[Visualization Placeholder]**: Bar chart comparing CV MAE of both models with error bars  
_Source: `notebooks/power_plant_prediction_en.ipynb`_

---

### 4. Model Evaluation

#### 4.1 Evaluation Metrics

| Metric | MAE | R² |
|--------|-----|----|
| Meaning | Average prediction error | Variance explained by model |
| Unit | MW | Unitless (0-1) |
| Goal | Lower is better | Higher is better |
| Example | 2.33 MW avg error | R² = 0.96 |

- MAE chosen for interpretability, robustness to outliers, and practical relevance.  
- R² included for standardized comparison.

#### 4.2 Test Set Results

- **Best model**: Random Forest  
- **Test MAE**: 2.33 MW  
- **Test R²**: 0.9637  

**Interpretation**:

1. Average prediction error ≈ 0.5% of output range  
2. Explains 96.37% of power output variance  
3. CV MAE close to test MAE → no overfitting  

**Visualization**: Scatter plot of actual vs predicted values shows most points tightly aligned along y=x line, indicating high accuracy and stability.

📊 **[Visualization Placeholder]**: Scatter plot of Actual vs Predicted values on test set  
_Source: `notebooks/power_plant_prediction_en.ipynb`_

---

### 5. Model Interpretation

#### 5.1 Why Random Forest Performs Better

1. Captures non-linear relationships in complex thermodynamic processes  
2. Handles feature interactions (e.g., high temp + high humidity effects)  
3. Ensemble approach averages 100 decision trees to reduce overall error

#### 5.2 Practical Implications

- **Energy Scheduling**: Predict output based on weather forecasts  
- **Operations Optimization**: Detect equipment issues when actual vs predicted differ  
- **Cost Control**: Improve fuel procurement and inventory management

- Test MAE 2.33 MW → practical accuracy achieved

#### 5.3 Overfitting Prevention

- Independent test set  
- 5-Fold CV ensures robust performance  
- Consistent MAE between CV and test set confirms generalization

---

### 6. Future Improvements

1. Explore additional algorithms: Gradient Boosting, Neural Networks  
2. Hyperparameter tuning for Random Forest  
3. Incorporate more features: historical generation data, equipment runtime

---

### 7. Conclusion

This project demonstrates a successful power plant energy prediction model:

- Correct problem identification (regression) and algorithm selection  
- Rigorous validation (5-Fold CV + independent test set)  
- Practical performance (Test MAE 2.33 MW, 0.5% relative error) suitable for real-world energy scheduling and operations optimization

---

## 中文

**作者**: Lily Wang  
**專案類型**: AI 產品管理 / 機器學習  
**演算法**: Linear Regression、Random Forest Regressor  

---

### 1. 專案目標

本專案旨在預測複合循環發電廠的淨電力輸出（PE）。準確的預測對於優化電廠營運、能源調度與維護規劃具有實務重要性。

專案聚焦於四個主要面向：

1. 建模方法
2. 模型建立
3. 模型評估
4. 模型解釋

---

### 2. 建模方法

#### 2.1 問題類型

- **目標變數**: PE（淨電力輸出），單位為 MW（百萬瓦特）  
- **數值範圍**: 420.26 – 495.76 MW  
- **問題類型**: 迴歸（預測連續數值）

📊 **[視覺化占位符]**: 資料集預覽，顯示 PE 欄位與數值範圍  
_來源: `notebooks/power_plant_prediction.ipynb`_

#### 2.2 特徵選擇

使用四個環境感測器特徵：

| 特徵 | 說明 | 範圍 | 與 PE 的相關性 |
|-----|------|------|---------------|
| AT | 環境溫度 | 1.81 – 37.11 °C | -0.948（強負相關）|
| V | 排氣真空度 | 25.36 – 81.56 cm Hg | -0.870（強負相關）|
| AP | 環境壓力 | 992.89 – 1033.30 millibar | +0.518（中度正相關）|
| RH | 相對濕度 | 25.56% – 100.16% | +0.390（弱正相關）|

**特徵選擇理由**：

1. 物理意義明確：每個特徵都基於熱力學原理影響電力輸出  
2. 與目標變數顯著相關  
3. 資料品質良好：9568 筆記錄，無缺失值或重複值

📊 **[視覺化占位符]**: 特徵相關性矩陣或熱圖  
_來源: `notebooks/power_plant_prediction.ipynb`_

#### 2.3 演算法選擇

比較兩種演算法：

1. **Linear Regression（線性迴歸）**  
   - 參數化演算法，假設線性關係  
   - 簡單、快速、易解釋  
   - 作為基準模型使用

2. **Random Forest Regressor（隨機森林迴歸）**  
   - 非參數化集成演算法（多棵決策樹）  
   - 捕捉非線性關係與特徵交互作用  
   - 不需假設資料分佈

---

### 3. 模型建立

#### 3.1 資料分割策略

- **資料集大小**: 9568 筆記錄  
- **初始分割**: 80% 訓練集（~7654 筆）、20% 測試集（~1914 筆）  
- **原則**: 測試集保留至最終評估，避免偏差

📊 **[視覺化占位符]**: 資料分割示意圖（80/20 + 5-Fold CV 說明）  
_來源: `docs/presentation_script.md` 或自製圖表_

#### 3.2 交叉驗證

- **方法**: 在訓練集上進行 5-Fold 交叉驗證  
- **目的**:  
  - 最大化資料使用  
  - 獲得穩健的評估結果  
  - 降低過擬合風險

#### 3.3 模型比較

- **評估指標**: MAE（平均絕對誤差）  
- **Scikit-learn 說明**: `cross_val_score` 使用 neg_mean_absolute_error 以符合「數值越高越好」的慣例

**5-Fold CV 結果**：

| 模型 | 平均 MAE | 標準差 | 5-Fold 範圍 |
|-----|----------|--------|-------------|
| Linear Regression | 3.63 MW | ±0.06 | 3.56–3.71 MW |
| Random Forest | 2.48 MW | ±0.07 | 2.35–2.55 MW |

**觀察結果**: Random Forest 誤差降低 31.68%，表現穩定

📊 **[視覺化占位符]**: 兩模型 CV MAE 比較長條圖（含誤差線）  
_來源: `notebooks/power_plant_prediction.ipynb`_

---

### 4. 模型評估

#### 4.1 評估指標

| 指標 | MAE | R² |
|-----|-----|----|
| 意義 | 平均預測誤差 | 模型解釋的變異比例 |
| 單位 | MW | 無單位（0-1）|
| 目標 | 越低越好 | 越高越好 |
| 範例 | 2.33 MW 平均誤差 | R² = 0.96 |

- 選擇 MAE：直觀易懂、對異常值穩健、符合實務需求  
- 同時報告 R²：標準化比較

#### 4.2 測試集結果

- **最佳模型**: Random Forest  
- **測試集 MAE**: 2.33 MW  
- **測試集 R²**: 0.9637  

**結果解讀**：

1. 平均預測誤差 ≈ 輸出範圍的 0.5%  
2. 解釋 96.37% 的電力輸出變異  
3. CV MAE 接近測試集 MAE → 無過擬合  

**視覺化說明**: 實際值 vs 預測值散佈圖顯示多數點緊密分佈在 y=x 線附近，顯示高準確度與穩定性

📊 **[視覺化占位符]**: 測試集的實際值 vs 預測值散佈圖  
_來源: `notebooks/power_plant_prediction.ipynb`_

---

### 5. 模型解釋

#### 5.1 為何 Random Forest 表現更佳

1. 捕捉複雜熱力學過程中的非線性關係  
2. 處理特徵交互作用（例如高溫 + 高濕度效應）  
3. 集成方法平均 100 棵決策樹以降低整體誤差

#### 5.2 實務應用

- **能源調度**: 根據天氣預報預測輸出  
- **運維優化**: 預測值與實際值差異可偵測設備問題  
- **成本控制**: 改善燃料採購與庫存管理

- 測試集 MAE 2.33 MW → 達到實用精度

#### 5.3 過擬合預防

- 獨立測試集  
- 5-Fold CV 確保穩健表現  
- CV 與測試集 MAE 一致性確認泛化能力

---

### 6. 未來改進方向

1. 探索其他演算法：Gradient Boosting、神經網路  
2. Random Forest 超參數調整  
3. 納入更多特徵：歷史發電數據、設備運轉時間

---

### 7. 結論

本專案展示了成功的電廠能源預測模型：

- 正確的問題識別（迴歸）與演算法選擇  
- 嚴謹的驗證（5-Fold CV + 獨立測試集）  
- 實用的性能（測試集 MAE 2.33 MW，相對誤差 0.5%），適用於實際能源調度與營運優化
