# Power Plant Energy Output Prediction (AI PM Portfolio Project)

[English](#english) | [中文](#中文)

---

## English

Machine learning project demonstrating predictive modeling and feature interpretation for **Combined Cycle Power Plant (CCPP) energy output forecasting**, developed as part of the **[Duke AI Product Management](https://www.coursera.org/specializations/ai-product-management-duke)** course. The project compares **Linear Regression** and **Random Forest** models for real-world hourly energy prediction.

### Project Summary

**Goal:** Predict net hourly electrical energy output (**PE**) of a Combined Cycle Power Plant using environmental sensor features.

**PM / Product Value:** Accurate short-term energy forecasts support operational planning, dispatch scheduling, maintenance indicators, and resource optimization. This project also demonstrates the value of machine learning from a Product Manager perspective, showing how predictive insights can improve operational decisions and resource efficiency.

**Dataset:** 9,568 hourly environmental records from the [UCI Machine Learning Repository](https://archive.ics.uci.edu/ml/datasets/combined+cycle+power+plant).

**Target & Unit:**
- **PE (Net Hourly Electrical Energy Output)** in **MW (MegaWatt)**
- Therefore, **MAE** is also reported in **MW**

### Modeling Workflow

1. Data exploration and feature selection  
2. Train/test split (80%/20%) for unbiased evaluation  
3. Model comparison using 5-Fold cross-validation  
4. Final evaluation on held-out test set  
5. Model interpretation and performance analysis  

### Key Results

**Cross-Validation (5-Fold on Training Set)**

| Model | Avg MAE (MW) | Std (MW) |
|------|--------------:|---------:|
| Linear Regression | 3.63 | 0.06 |
| Random Forest | 2.48 | 0.07 |

**Final Test Set (Held-out 20%)**

| Model | Test MAE (MW) | Test R² |
|------|---------------:|--------:|
| Linear Regression | 3.60 | 0.9301 |
| Random Forest | 2.33 | 0.9637 |

**Decision:** Random Forest selected as final model.  
- Test MAE reduced 35.28% (3.60 → 2.33 MW)  
- CV MAE (2.48) close to test MAE, indicating low risk of overfitting  

**PM Insight:** ~35% MAE improvement enhances reliability of energy forecasting, supporting better operational decisions and reducing inefficiencies.

---

### Features

| Feature | Description | Range |
|---------|------------|-------|
| **AT** | Ambient Temperature (°C) | 1.81 ~ 37.11 |
| **V** | Exhaust Vacuum (cm Hg) | 25.36 ~ 81.56 |
| **AP** | Ambient Pressure (millibar) | 992.89 ~ 1033.30 |
| **RH** | Relative Humidity (%) | 25.56 ~ 100.16 |

---

### Repository Structure
```
ai-pm-machine-learning-project/
├── notebooks/
│   ├── power_plant_prediction.ipynb        # Chinese (learning/notes)
│   └── power_plant_prediction_en.ipynb     # English (submission)
├── docs/
│   ├── project_details.md                  # Project overview (bilingual)
│   └── model_analysis.md                   # Methodology (bilingual)
├── main.py
├── pyproject.toml
├── uv.lock
└── README.md
```

### Documentation

- **[project_details.md](docs/project_details.md)**: Comprehensive project overview with detailed methodology, evaluation results, and visualization placeholders (bilingual)
- **Notebooks**: Interactive analysis and model implementation
  - [power_plant_prediction_en.ipynb](notebooks/power_plant_prediction_en.ipynb) - English version
  - [power_plant_prediction.ipynb](notebooks/power_plant_prediction.ipynb) - Chinese version
- **[model_analysis.md](docs/model_analysis.md)**: Detailed technical methodology (bilingual)

### How to Run

Requires **Python 3.12** and **uv** for dependency management.

**Note:** Download the [CCPP dataset](https://archive.ics.uci.edu/ml/datasets/combined+cycle+power+plant) and save as `CCPP_data.csv` in the project root before running.

```bash
# Install dependencies
uv sync

# Activate virtual environment
source .venv/bin/activate

# Launch Jupyter Notebook
jupyter notebook
```

Open:
- `notebooks/power_plant_prediction_en.ipynb` (English)
- `notebooks/power_plant_prediction.ipynb` (Chinese)

### Notes

- This repository is only for educational/portfolio use.

---

## 中文

本專案為 **[Duke AI Product Management](https://www.coursera.org/specializations/ai-product-management-duke)** 課程的 AI 導向機器學習專案，比較 **Linear Regression** 與 **Random Forest**，用於預測真實情境下複合循環發電廠的**每小時淨電力輸出**。

### 專案摘要

**目標**：使用環境感測器特徵預測複合循環發電廠的淨電力輸出（**PE**）。

**PM 觀點**：更準確的短期發電量預測可支援規劃與營運決策（例如：調度規劃、維護指標、資源配置），本專案同時展示了從產品經理角度應用機器學習的價值，說明如何透過預測洞察支援營運決策與資源優化。

**資料集**：9,568 筆每小時資料，來自 [UCI Machine Learning Repository](https://archive.ics.uci.edu/ml/datasets/combined+cycle+power+plant) 的 CCPP 資料集（不包含在 repo 中）。

**目標與單位**：
- **PE (Net Hourly Electrical Energy Output)** 單位為 **MW（MegaWatt，百萬瓦特）**。
- 因此 **MAE** 的單位也為 **MW**。

## 建模流程

1. 資料探索與特徵選擇  
2. 訓練/測試切分 (80% / 20%)  
3. 模型比較，使用 5-Fold 交叉驗證  
4. 最終測試集評估  
5. 模型解釋與性能分析  

### 主要結果

模型比較採用：
- **訓練/測試切分**：80% / 20%（測試集保留到最後才使用）
- **交叉驗證**：在訓練集上做 5-Fold CV
- **指標**：MAE（越低越好）與 R²（越高越好）

**交叉驗證（Training Set 上的 5-Fold CV）**

| 模型 | 平均 MAE (MW) | 標準差 (MW) |
|------|--------------:|------------:|
| Linear Regression | 3.63 | 0.06 |
| Random Forest | 2.48 | 0.07 |

**最終測試集（保留的 20%）**

| 模型 | 測試集 MAE (MW) | 測試集 R² |
|------|----------------:|----------:|
| Linear Regression | 3.60 | 0.9301 |
| Random Forest | 2.33 | 0.9637 |

**結論（模型選擇）**：Random Forest 作為最終模型。
- 測試集 MAE 由 3.60 → 2.33 MW（降低 35.28%）
- CV MAE (2.48) 與測試集 MAE (2.33) 接近，未見明顯過擬合

**PM 見解**：35% 的 MAE 改善提高了預測可靠性，有助於更好的營運決策，降低資源浪費。

### 特徵說明

使用特徵：
- **AT (Ambient Temperature)**：1.81°C ~ 37.11°C
- **V (Exhaust Vacuum)**：25.36 ~ 81.56 cm Hg
- **AP (Ambient Pressure)**：992.89 ~ 1033.30 millibar
- **RH (Relative Humidity)**：25.56% ~ 100.16%

### 專案結構

```
ai-pm-machine-learning-project/
├── notebooks/
│   ├── power_plant_prediction.ipynb        # 中文（學習/註解）
│   └── power_plant_prediction_en.ipynb     # 英文（提交）
├── docs/
│   ├── project_details.md                  # 專案概覽（雙語）
│   └── model_analysis.md                   # 方法論（雙語）
├── main.py
├── pyproject.toml
├── uv.lock
└── README.md
```

### 文件說明

- **[project_details.md](docs/project_details.md)**：完整專案概覽，含詳細方法論、評估結果與視覺化占位符（雙語）
- **Notebooks**：互動式分析與模型實作
  - [power_plant_prediction_en.ipynb](notebooks/power_plant_prediction_en.ipynb) - 英文
  - [power_plant_prediction.ipynb](notebooks/power_plant_prediction.ipynb) - 中文
- **[model_analysis.md](docs/model_analysis.md)**：詳細技術方法論（雙語）

### 如何執行

本專案使用 **Python 3.12** 與 **uv** 管理依賴。

**注意**：執行前請先下載 [CCPP 資料集](https://archive.ics.uci.edu/ml/datasets/combined+cycle+power+plant)，並儲存為專案根目錄的 `CCPP_data.csv`。

```bash
uv sync
source .venv/bin/activate
jupyter notebook
```

建議開啟：
- `notebooks/power_plant_prediction_en.ipynb`（英文）
- `notebooks/power_plant_prediction.ipynb`（中文）

### 備註

- 本專案僅供學習與作品集展示使用。
