# WHO Life Expectancy — Feature Selection & Predictive Modelling

Systematic feature selection and predictive modelling on WHO global health data — identifying which combination of variables best predicts life expectancy across 183 countries, using three independent selection methods and four model types.

👉 **[View the live analytical report](https://larawahbi.github.io/life-expectancy-feature-selection/who_life_expectancy_report.html)**

---

## About

Built by [Lara Amro](https://www.linkedin.com/in/laraamro/) as a self-directed portfolio project in applied machine learning and statistical analysis. The project was designed to address a key gap in exploratory work — formally testing which variables matter and comparing model performance systematically, rather than assuming feature relevance from correlation alone.

**Background:** 8+ years in data engineering, business intelligence, and data governance across international organisations including UNHCR, WFP, and IFRC.

---

## Key Findings

### 🔬 Feature Selection — 3 Methods, 9 Agreed
Three independent selection approaches — correlation analysis, forward stepwise regression, and LASSO — were applied to 15 candidate variables. **9 features reached unanimous agreement** across all three methods. 3 were unanimously dropped (infant deaths, Hepatitis B, Total expenditure).

### 🌲 Random Forest wins by a large margin

| Model | R² | RMSE |
|---|---|---|
| Linear Regression | 0.809 | 4.16 years |
| Ridge Regression | 0.809 | 4.16 years |
| **Random Forest** | **0.960** | **1.89 years** |
| Gradient Boosting | 0.940 | 2.33 years |

The 15-point R² gap confirms significant non-linear relationships that straight-line models cannot capture.

### 🔴 HIV/AIDS dominates — but not how you'd expect
HIV/AIDS ranked 5th in individual correlation but accounted for **58.8% of the Random Forest's predictive power**. This reveals a threshold effect invisible to linear analysis: the disease barely affects life expectancy in most countries but is catastrophic in a concentrated group of sub-Saharan African nations.

### 📍 Where the model struggled
The largest prediction errors cluster around three structural patterns:
- **Conflict** — South Sudan 2000: +11.0 years overestimate. No variable captures active war or health system collapse.
- **Rapid recovery** — Zimbabwe 2015: −8.2 years underestimate. The model learned Zimbabwe as low life expectancy and missed the post-HIV/AIDS treatment recovery.
- **Outlier efficiency** — Singapore 2006: −7.3 years underestimate. Exceptional governance produces outcomes beyond what GDP and schooling predict.

---

## Project Structure

```
life-expectancy-analysis/
│
├── data/                              ← not included, see instructions below
│   └── README.md                      ← data download instructions
│
├── notebooks/
│   ├── 01_exploratory_analysis.ipynb  ← data structure, missing values, correlations
│   ├── 02_feature_selection.ipynb     ← correlation, stepwise, LASSO comparison
│   ├── 03_model_comparison.ipynb      ← four models, cross-validated performance
│   └── 04_report.ipynb               ← summary dashboard and HTML report generation
│
├── docs/
│   └── who_life_expectancy_report.html ← live analytical report (GitHub Pages)
│
├── outputs/                           ← generated charts, not included in repo
├── requirements.txt
└── README.md
```

---

## Dataset

WHO Global Health Observatory data combined with UN economic data. Originally published on Kaggle by kumarajarshi.

Download from Kaggle:
https://www.kaggle.com/datasets/kumarajarshi/life-expectancy-who

Once downloaded, place `Life Expectancy Data.csv` in the `data/` folder.

**Coverage:** 183 countries · 2000–2015 · 22 variables across immunisation, mortality, economic, and social domains.

---

## How to Run

**1. Clone the repository**
```bash
git clone https://github.com/larawahbi/life-expectancy-feature-selection.git
cd life-expectancy-feature-selection
```

**2. Create and activate a virtual environment**
```bash
python3 -m venv venv
source venv/bin/activate
```

**3. Install dependencies**
```bash
pip install -r requirements.txt
```

**4. Register the kernel with Jupyter**
```bash
python -m ipykernel install --user --name=venv --display-name "Python (life-exp)"
```

**5. Download the dataset** and place it in the `data/` folder as described above.

**6. Run the notebooks in order** — 01 through 04. Each notebook saves its outputs for the next to pick up.

---

## Methods

**Feature selection:**
- Pearson correlation with threshold analysis
- Forward stepwise regression with 5-fold cross-validation
- LASSO regression with LassoCV automatic alpha selection

**Model comparison:**
- Linear Regression, Ridge Regression, Random Forest, Gradient Boosting
- 5-fold cross-validation throughout
- Evaluated on R², RMSE, and MAE

**Final model:** Random Forest (100 estimators) trained on 80% of data, evaluated on held-out 20% test set.

---

## Tools and Libraries

- Python 3.12
- pandas, numpy
- scikit-learn
- statsmodels
- matplotlib, seaborn
- Jupyter / VS Code

---

## Limitations

- Data covers 2000–2015. Post-2015 dynamics including COVID-19 are not represented.
- Missing data concentrated in smaller countries was imputed using group medians — introducing uncertainty for those observations.
- The model cannot capture conflict, governance quality, or health system efficiency — the primary drivers of its largest prediction errors.
- Random Forest feature importance can be unstable when correlated predictors are present. The Schooling vs Income composition result should be interpreted with this in mind.

---

## Data Source

World Health Organization Global Health Observatory via Kaggle.
Original dataset: © WHO · For research and educational use.
https://www.kaggle.com/datasets/kumarajarshi/life-expectancy-who
