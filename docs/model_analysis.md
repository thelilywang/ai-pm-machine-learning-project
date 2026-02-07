# Model Methodology / 模型方法論

[English](#english) | [中文](#中文)

---

## English

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

---

## 中文

# 模型方法論詳細說明

## 1. 問題類型判斷：為何是迴歸問題？

### 迴歸 vs 分類的區別

**分類問題 (Classification)**：預測的目標是**離散的類別標籤**。
- 例如：預測電子郵件是否為垃圾郵件（是/否）
- 例如：預測客戶會購買哪種產品（A類/B類/C類）

**迴歸問題 (Regression)**：預測的目標是**連續的數值**。
- 例如：預測房價（$100,000 到 $1,000,000）
- 例如：預測溫度（-10°C 到 40°C）

### 本專案的判斷

在本專案中，我們要預測的目標變數是 **PE (Net Hourly Electrical Energy Output)**：
- **資料類型**：連續數值
- **數值範圍**：420.26 ~ 495.76 **MW (MegaWatt 百萬瓦特)**
- **特性**：可以取任何在此範圍內的數值，例如 445.23 MW、478.91 MW 等
- **單位說明**：因為目標變數 PE 的單位是 MW，所以預測誤差 MAE 的單位也是 MW

**結論**：由於目標變數 PE 是一個連續的數值變數，而非離散的類別，因此這是一個**迴歸問題（Regression Problem）**。

---

## 2. 特徵選擇：為何使用這四個特徵？

### 選用的特徵

本專案使用所有四個環境感測器特徵：

1. **AT (Ambient Temperature)** - 環境溫度 (1.81°C ~ 37.11°C)
2. **V (Exhaust Vacuum)** - 排氣真空度 (25.36 ~ 81.56 cm Hg)
3. **AP (Ambient Pressure)** - 環境壓力 (992.89 ~ 1033.30 millibar)
4. **RH (Relative Humidity)** - 相對濕度 (25.56% ~ 100.16%)

### 選擇理由

#### 2.1 物理意義明確
這些特徵都是實際的環境感測器數據，直接影響發電廠的運作效率：
- **環境溫度**：影響冷卻系統效率和熱力循環
- **排氣真空度**：反映蒸汽渦輪機的效率
- **環境壓力**：影響空氣密度和燃燒效率
- **相對濕度**：影響冷卻系統性能

#### 2.2 與目標變數的相關性
從相關性分析可以看出，所有特徵都與 PE（電力輸出）有顯著相關：
- **AT（溫度）**：負相關最強（溫度越高，輸出越低）
- **V（真空度）**：負相關（真空度越大，輸出越低）
- **AP（壓力）**：正相關（壓力越高，輸出越高）
- **RH（濕度）**：負相關（濕度越高，輸出越低）

#### 2.3 資料品質良好
- 無缺失值
- 數值範圍合理
- 無明顯異常值

**結論**：四個特徵都具有實務意義且與目標變數相關，因此全部納入模型。

---

## 3. 演算法選擇：為何選這兩種方法？

### 3.1 Linear Regression（線性迴歸）

#### 特性
- **參數化演算法（Parametric Algorithm）**
- 假設特徵與目標變數之間存在**線性關係**
- 公式：PE = β₀ + β₁×AT + β₂×V + β₃×AP + β₄×RH

#### 選擇理由
1. **作為基準模型（Baseline Model）**：簡單且易於解釋，適合作為比較的基準
2. **運算效率高**：訓練速度快，適合快速驗證
3. **可解釋性強**：每個特徵的係數可以直接解釋其對目標的影響
4. **參數少**：不易過擬合（overfitting）

#### 適用情境
- 當特徵與目標之間的關係較為線性時
- 當需要快速建立初步模型時
- 當需要模型具有高可解釋性時

### 3.2 Random Forest Regressor（隨機森林迴歸）

#### 特性
- **非參數化集成演算法（Non-parametric Ensemble Algorithm）**
- 由多棵決策樹組成，透過投票或平均來預測
- 不需要假設資料的分佈形式

#### 選擇理由
1. **處理非線性關係**：能自動捕捉特徵之間的複雜交互作用
2. **抗過擬合能力強**：透過集成多棵樹來降低過擬合風險
3. **對特徵尺度不敏感**：不需要標準化或正規化
4. **提供特徵重要性**：可以評估哪些特徵對預測最重要

#### 適用情境
- 當特徵與目標之間的關係較為複雜、非線性時
- 當特徵之間可能存在交互作用時
- 當資料量足夠大（本專案有 9568 筆）時

### 3.3 為何比較這兩種演算法？

| 特性 | Linear Regression | Random Forest |
|------|-------------------|---------------|
| 模型類型 | 參數化 | 非參數化 |
| 複雜度 | 簡單 | 複雜 |
| 訓練速度 | 快 | 較慢 |
| 處理非線性 | 弱 | 強 |
| 過擬合風險 | 低 | 中（透過集成降低） |
| 可解釋性 | 高 | 中 |

**結論**：透過比較**簡單的參數化模型**與**複雜的非參數化模型**，可以評估資料的複雜度，並選擇最適合的預測方法。

---

## 4. 評估指標：為何選擇 MAE？

### MAE (Mean Absolute Error) 的定義

$$\text{MAE} = \frac{1}{n} \sum_{i=1}^{n} |y_i - \hat{y}_i|$$

其中：
- $y_i$ 是實際值
- $\hat{y}_i$ 是預測值
- $n$ 是樣本數量

### 選擇 MAE 的理由

#### 4.1 直觀易懂
- MAE 的單位與目標變數相同（MW）
- 可以直接解讀：「平均預測誤差為 X MW」
- 對於非技術背景的利害關係人更容易理解

#### 4.2 對異常值較穩健（Robust to Outliers）
- MAE 使用絕對值，不會過度放大大誤差的影響
- 相比之下，MSE/RMSE 使用平方，會過度懲罰極端誤差

#### 4.3 反映實務需求
在電廠管理中，我們關心的是：
- 「平均來說，我們的預測會偏離實際值多少？」
- MAE 直接回答這個問題

### MAE vs 其他指標

| 指標 | 優點 | 缺點 |
|------|------|------|
| **MAE** | 直觀、對異常值穩健 | 數學上不可微分於零點 |
| **MSE** | 數學性質好、可微分 | 單位不直觀（MW²） |
| **RMSE** | 單位與目標相同 | 對異常值敏感 |
| **R²** | 顯示解釋比例 | 不反映實際誤差大小 |

**結論**：MAE 是最適合本專案的主要評估指標，但也會輔助報告 R² 來說明模型的解釋能力。

---

## 5. 驗證策略：為何使用 5-Fold Cross-Validation？

### 5.1 資料分割策略

#### Initial Split（初始分割）
- **訓練集**：80%（約 7654 筆）
- **測試集**：20%（約 1914 筆）

**目的**：
- 訓練集用於模型訓練和比較
- 測試集**完全保留**，僅在最後用於評估最終模型
- 確保測試結果**無偏且可靠**

#### K-Fold Cross-Validation（K 折交叉驗證）
- 將訓練集分成 **K=5** 份
- 每次用 4 份訓練、1 份驗證
- 重複 5 次，確保每份資料都被用作驗證集一次

### 5.2 為何選擇 5-Fold？

#### 優點
1. **充分利用訓練資料**：每次使用 80% 的訓練集（約 6123 筆）來訓練
2. **評估更穩健**：透過 5 次驗證的平均結果，降低單次分割的隨機性
3. **計算成本適中**：相比 10-Fold，訓練次數較少但仍有足夠的驗證可靠度
4. **適合中型資料集**：對於 7654 筆訓練資料，5-Fold 是常見且有效的選擇

#### 與其他方法的比較

| 方法 | 訓練資料使用率 | 可靠度 | 計算成本 |
|------|---------------|--------|----------|
| **Hold-out (70/30)** | 70% | 低（單次分割） | 低 |
| **3-Fold CV** | 66.7% | 中 | 低 |
| **5-Fold CV** | 80% | 高 | 中 |
| **10-Fold CV** | 90% | 很高 | 高 |
| **LOOCV** | 99.9% | 最高 | 極高 |

**結論**：5-Fold CV 在資料利用率、可靠度和計算成本之間取得最佳平衡。

---

## 6. 如何避免過擬合（Overfitting）？

### 6.1 什麼是過擬合？

**過擬合**：模型在訓練資料上表現很好，但在未見過的測試資料上表現很差。

### 6.2 本專案的防範措施

#### 措施 1：保留獨立測試集
- 測試集**從未被用於模型訓練或選擇**
- 確保最終評估結果**無偏**

#### 措施 2：使用交叉驗證
- 5-Fold CV 評估模型在不同資料子集上的**泛化能力**
- 避免模型只是「記住」特定的驗證集

#### 措施 3：比較簡單與複雜模型
- Linear Regression：低複雜度，低過擬合風險
- Random Forest：雖較複雜，但透過集成降低過擬合

#### 措施 4：Random Forest 的內建機制
- **Bootstrap Aggregating (Bagging)**：每棵樹用不同的資料子集訓練
- **隨機特徵選擇**：每次分裂只考慮部分特徵
- **n_estimators=100**：集成 100 棵樹，降低單棵樹的過擬合

### 6.3 如何判斷是否過擬合？

觀察指標：
- **訓練集 MAE << 驗證集 MAE**：可能過擬合
- **交叉驗證 MAE ≈ 測試集 MAE**：泛化能力良好

---

## 7. 模型選擇流程總結

```
原始資料 (9568 筆)
    ↓
初始分割 (80/20)
    ↓
訓練集 (80%)              測試集 (20%) ← 完全保留
    ↓
5-Fold 交叉驗證
    ↓
比較 Linear Regression vs Random Forest
    ↓
選擇平均 MAE 最低的模型
    ↓
在完整訓練集上重新訓練
    ↓
在測試集上計算最終 MAE 和 R²
    ↓
報告最終結果
```

---

## 8. 結論

### 為何這個方法論是合理的？

1. **問題定義正確**：識別為迴歸問題，選擇適當的演算法和指標
2. **特徵選擇有理**：所有特徵都具有物理意義且與目標相關
3. **演算法多樣性**：比較參數化與非參數化方法，評估資料複雜度
4. **評估指標適當**：MAE 直觀且符合實務需求
5. **驗證策略嚴謹**：使用交叉驗證和獨立測試集，確保結果可靠
6. **避免過擬合**：透過多種機制確保模型的泛化能力

這個方法論遵循**機器學習的最佳實務（Best Practices）**，確保模型的可靠性和實用性。


## 1. 問題類型判斷：為何是迴歸問題？

### 迴歸 vs 分類的區別

**分類問題 (Classification)**：預測的目標是**離散的類別標籤**。
- 例如：預測電子郵件是否為垃圾郵件（是/否）
- 例如：預測客戶會購買哪種產品（A類/B類/C類）

**迴歸問題 (Regression)**：預測的目標是**連續的數值**。
- 例如：預測房價（$100,000 到 $1,000,000）
- 例如：預測溫度（-10°C 到 40°C）

### 本專案的判斷

在本專案中，我們要預測的目標變數是 **PE (Net Hourly Electrical Energy Output)**：
- **資料類型**：連續數值
- **數值範圍**：420.26 ~ 495.76 **MW (MegaWatt 百萬瓦特)**
- **特性**：可以取任何在此範圍內的數值，例如 445.23 MW、478.91 MW 等
- **單位說明**：因為目標變數 PE 的單位是 MW，所以預測誤差 MAE 的單位也是 MW

**結論**：由於目標變數 PE 是一個連續的數值變數，而非離散的類別，因此這是一個**迴歸問題（Regression Problem）**。

---

## 2. 特徵選擇：為何使用這四個特徵？

### 選用的特徵

本專案使用所有四個環境感測器特徵：

1. **AT (Ambient Temperature)** - 環境溫度 (1.81°C ~ 37.11°C)
2. **V (Exhaust Vacuum)** - 排氣真空度 (25.36 ~ 81.56 cm Hg)
3. **AP (Ambient Pressure)** - 環境壓力 (992.89 ~ 1033.30 millibar)
4. **RH (Relative Humidity)** - 相對濕度 (25.56% ~ 100.16%)

### 選擇理由

#### 2.1 物理意義明確
這些特徵都是實際的環境感測器數據，直接影響發電廠的運作效率：
- **環境溫度**：影響冷卻系統效率和熱力循環
- **排氣真空度**：反映蒸汽渦輪機的效率
- **環境壓力**：影響空氣密度和燃燒效率
- **相對濕度**：影響冷卻系統性能

#### 2.2 與目標變數的相關性
從相關性分析可以看出，所有特徵都與 PE（電力輸出）有顯著相關：
- **AT（溫度）**：負相關最強（溫度越高，輸出越低）
- **V（真空度）**：負相關（真空度越大，輸出越低）
- **AP（壓力）**：正相關（壓力越高，輸出越高）
- **RH（濕度）**：負相關（濕度越高，輸出越低）

#### 2.3 資料品質良好
- 無缺失值
- 數值範圍合理
- 無明顯異常值

**結論**：四個特徵都具有實務意義且與目標變數相關，因此全部納入模型。

---

## 3. 演算法選擇：為何選這兩種方法？

### 3.1 Linear Regression（線性迴歸）

#### 特性
- **參數化演算法（Parametric Algorithm）**
- 假設特徵與目標變數之間存在**線性關係**
- 公式：PE = β₀ + β₁×AT + β₂×V + β₃×AP + β₄×RH

#### 選擇理由
1. **作為基準模型（Baseline Model）**：簡單且易於解釋，適合作為比較的基準
2. **運算效率高**：訓練速度快，適合快速驗證
3. **可解釋性強**：每個特徵的係數可以直接解釋其對目標的影響
4. **參數少**：不易過擬合（overfitting）

#### 適用情境
- 當特徵與目標之間的關係較為線性時
- 當需要快速建立初步模型時
- 當需要模型具有高可解釋性時

### 3.2 Random Forest Regressor（隨機森林迴歸）

#### 特性
- **非參數化集成演算法（Non-parametric Ensemble Algorithm）**
- 由多棵決策樹組成，透過投票或平均來預測
- 不需要假設資料的分佈形式

#### 選擇理由
1. **處理非線性關係**：能自動捕捉特徵之間的複雜交互作用
2. **抗過擬合能力強**：透過集成多棵樹來降低過擬合風險
3. **對特徵尺度不敏感**：不需要標準化或正規化
4. **提供特徵重要性**：可以評估哪些特徵對預測最重要

#### 適用情境
- 當特徵與目標之間的關係較為複雜、非線性時
- 當特徵之間可能存在交互作用時
- 當資料量足夠大（本專案有 9568 筆）時

### 3.3 為何比較這兩種演算法？

| 特性 | Linear Regression | Random Forest |
|------|-------------------|---------------|
| 模型類型 | 參數化 | 非參數化 |
| 複雜度 | 簡單 | 複雜 |
| 訓練速度 | 快 | 較慢 |
| 處理非線性 | 弱 | 強 |
| 過擬合風險 | 低 | 中（透過集成降低） |
| 可解釋性 | 高 | 中 |

**結論**：透過比較**簡單的參數化模型**與**複雜的非參數化模型**，可以評估資料的複雜度，並選擇最適合的預測方法。

---

## 4. 評估指標：為何選擇 MAE？

### MAE (Mean Absolute Error) 的定義

$$\text{MAE} = \frac{1}{n} \sum_{i=1}^{n} |y_i - \hat{y}_i|$$

其中：
- $y_i$ 是實際值
- $\hat{y}_i$ 是預測值
- $n$ 是樣本數量

### 選擇 MAE 的理由

#### 4.1 直觀易懂
- MAE 的單位與目標變數相同（MW）
- 可以直接解讀：「平均預測誤差為 X MW」
- 對於非技術背景的利害關係人更容易理解

#### 4.2 對異常值較穩健（Robust to Outliers）
- MAE 使用絕對值，不會過度放大大誤差的影響
- 相比之下，MSE/RMSE 使用平方，會過度懲罰極端誤差

#### 4.3 反映實務需求
在電廠管理中，我們關心的是：
- 「平均來說，我們的預測會偏離實際值多少？」
- MAE 直接回答這個問題

### MAE vs 其他指標

| 指標 | 優點 | 缺點 |
|------|------|------|
| **MAE** | 直觀、對異常值穩健 | 數學上不可微分於零點 |
| **MSE** | 數學性質好、可微分 | 單位不直觀（MW²） |
| **RMSE** | 單位與目標相同 | 對異常值敏感 |
| **R²** | 顯示解釋比例 | 不反映實際誤差大小 |

**結論**：MAE 是最適合本專案的主要評估指標，但也會輔助報告 R² 來說明模型的解釋能力。

---

## 5. 驗證策略：為何使用 5-Fold Cross-Validation？

### 5.1 資料分割策略

#### Initial Split（初始分割）
- **訓練集**：80%（約 7654 筆）
- **測試集**：20%（約 1914 筆）

**目的**：
- 訓練集用於模型訓練和比較
- 測試集**完全保留**，僅在最後用於評估最終模型
- 確保測試結果**無偏且可靠**

#### K-Fold Cross-Validation（K 折交叉驗證）
- 將訓練集分成 **K=5** 份
- 每次用 4 份訓練、1 份驗證
- 重複 5 次，確保每份資料都被用作驗證集一次

### 5.2 為何選擇 5-Fold？

#### 優點
1. **充分利用訓練資料**：每次使用 80% 的訓練集（約 6123 筆）來訓練
2. **評估更穩健**：透過 5 次驗證的平均結果，降低單次分割的隨機性
3. **計算成本適中**：相比 10-Fold，訓練次數較少但仍有足夠的驗證可靠度
4. **適合中型資料集**：對於 7654 筆訓練資料，5-Fold 是常見且有效的選擇

#### 與其他方法的比較

| 方法 | 訓練資料使用率 | 可靠度 | 計算成本 |
|------|---------------|--------|----------|
| **Hold-out (70/30)** | 70% | 低（單次分割） | 低 |
| **3-Fold CV** | 66.7% | 中 | 低 |
| **5-Fold CV** | 80% | 高 | 中 |
| **10-Fold CV** | 90% | 很高 | 高 |
| **LOOCV** | 99.9% | 最高 | 極高 |

**結論**：5-Fold CV 在資料利用率、可靠度和計算成本之間取得最佳平衡。

---

## 6. 如何避免過擬合（Overfitting）？

### 6.1 什麼是過擬合？

**過擬合**：模型在訓練資料上表現很好，但在未見過的測試資料上表現很差。

### 6.2 本專案的防範措施

#### 措施 1：保留獨立測試集
- 測試集**從未被用於模型訓練或選擇**
- 確保最終評估結果**無偏**

#### 措施 2：使用交叉驗證
- 5-Fold CV 評估模型在不同資料子集上的**泛化能力**
- 避免模型只是「記住」特定的驗證集

#### 措施 3：比較簡單與複雜模型
- Linear Regression：低複雜度，低過擬合風險
- Random Forest：雖較複雜，但透過集成降低過擬合

#### 措施 4：Random Forest 的內建機制
- **Bootstrap Aggregating (Bagging)**：每棵樹用不同的資料子集訓練
- **隨機特徵選擇**：每次分裂只考慮部分特徵
- **n_estimators=100**：集成 100 棵樹，降低單棵樹的過擬合

### 6.3 如何判斷是否過擬合？

觀察指標：
- **訓練集 MAE << 驗證集 MAE**：可能過擬合
- **交叉驗證 MAE ≈ 測試集 MAE**：泛化能力良好

---

## 7. 模型選擇流程總結

```
原始資料 (9568 筆)
    ↓
初始分割 (80/20)
    ↓
訓練集 (80%)              測試集 (20%) ← 完全保留
    ↓
5-Fold 交叉驗證
    ↓
比較 Linear Regression vs Random Forest
    ↓
選擇平均 MAE 最低的模型
    ↓
在完整訓練集上重新訓練
    ↓
在測試集上計算最終 MAE 和 R²
    ↓
報告最終結果
```

---

## 8. 結論

### 為何這個方法論是合理的？

1. **問題定義正確**：識別為迴歸問題，選擇適當的演算法和指標
2. **特徵選擇有理**：所有特徵都具有物理意義且與目標相關
3. **演算法多樣性**：比較參數化與非參數化方法，評估資料複雜度
4. **評估指標適當**：MAE 直觀且符合實務需求
5. **驗證策略嚴謹**：使用交叉驗證和獨立測試集，確保結果可靠
6. **避免過擬合**：透過多種機制確保模型的泛化能力

這個方法論遵循**機器學習的最佳實務（Best Practices）**，確保模型的可靠性和實用性。
