# Marketing Mix Model for TV Show Viewership (Ridge & Bayesian MMMs)

> A comprehensive Marketing Mix Model that optimizes media spend across Network TV, Cable TV, and Digital channels, achieving **97.4% prediction accuracy** and identifying **$185k in revenue opportunity** through smarter budget allocation.

---

## 🎯 Project Overview

**The Question:** How should a TV network allocate its $14M annual media budget across three advertising channels to maximize viewership?

**The Answer:** By reallocating the existing budget (not spending more), we can gain **+264k viewers (+2.6%)** and **+$185k revenue**.

### Key Results

| Metric | Value |
|--------|-------|
| **Model Accuracy** | R² = 0.974 (explains 97.4% of variance) |
| **Prediction Error** | RMSE = 9,741 viewers (8.5% MAPE) |
| **Revenue Opportunity** | +$185,296 (budget reallocation only) |
| **Viewer Lift** | +264,336 viewers (+2.6%) |

---

## 📊 The Data

- **420 episode-weeks** across 8 TV shows (2020-2025)
- **3 advertising channels**: Network TV, Cable TV, Digital
- **~$14M total media spend** to optimize
- **Weekly viewership** ranging from 33k to 262k per episode

---

## 🔍 Methodology

### 1. Data Processing
Started with **3,143 daily-level rows** (7 days per episode-week) and aggregated to **420 episode-weeks**. Validated that aggregation preserved spend totals.

### 2. Feature Engineering
Created three types of media transformations:
- **Log transforms** → Capture diminishing returns
- **Adstock transforms** → Model carryover effects (viewers seeing ads this week → tuning in next week)
- **Hill saturation** → Model plateau at very high spend

Plus control variables: episode type (premiere/finale), previous episode viewership, holidays, lead-in bonuses, show/season fixed effects.

### 3. Model Specifications
Tested **three Ridge regression models**:

| Model | Features | Test R² | RMSE |
|-------|----------|---------|------|
| **A: Log** | Log(spend) + controls | **0.974** ✓ | **9,741** ✓ |
| B: Adstock | Log + Adstock + controls | 0.966 | 11,199 |
| C: Hill | Hill saturation + controls | 0.958 | 12,362 |

**Winner: Model A** (simplest, most accurate, most interpretable)

### 4. Validation
- **Train/Test Split:** 80/20 chronological (no future data leakage)
- **Cross-Validation:** 5-fold time-series CV to tune Ridge penalty
- **Bootstrap:** 200 iterations → 90% confidence intervals on all coefficients
- **Bayesian Validation:** Separate Bayesian MMM (R² = 0.965) confirms findings

---

## 📈 Key Findings

### Channel Effectiveness (with 90% Confidence Intervals)

All three channels drive viewership, but with different strengths:

| Channel | Impact Coefficient | 90% CI | ROI | Status |
|---------|-------------------|--------|-----|--------|
| **Cable TV** | 9,285 | [7,562 – 10,796] | 0.60x | ✓ Strongest |
| **Network TV** | 8,758 | [7,208 – 10,633] | 0.51x | ✓ Strong |
| **Digital** | 6,443 | [4,659 – 8,140] | 0.38x | ✓ Supporting |

**Interpretation:**
- All channels are **statistically significant** (90% CI excludes zero)
- TV (Network + Cable) provides **75% of media-driven lift**
- ROI < 1.0 means channels are **near saturation** → focus on reallocation, not increases

### The Biggest Driver: Momentum

**Last week's viewership** is the strongest predictor (coefficient ≈ 1.0):
- If 100k people watched last episode → expect ~100k this episode (holding media constant)
- This "habit effect" explains why keeping shows on the air matters

### Optimal Budget Allocation

| Channel | Current | Optimal | Change |
|---------|---------|---------|--------|
| **Network TV** | 24.3% | 35.0% | +10.7% ↑ |
| **Cable TV** | 36.9% | 37.5% | +0.6% → |
| **Digital** | 38.8% | 27.5% | -11.3% ↓ |

**Result:** +264k viewers, +$185k revenue, **$0 extra spend**

---

## 💻 Tech Stack

- **Python 3.8+** for all analysis
- **pandas** for data manipulation
- **scikit-learn** for Ridge regression, cross-validation
- **NumPy/SciPy** for statistical computations
- **matplotlib/seaborn** for visualizations
- **statsmodels** for OLS comparison and VIF analysis

---

## 📁 Repository Structure

```
marketing-mix-model/
│
├── Case Study - Taroon Ganesh.ipynb    # Main analysis notebook (complete pipeline)
└── README.md                  
```

---

## 🚀 How to Run

### Prerequisites
```bash
pip install pandas numpy scikit-learn matplotlib seaborn scipy statsmodels
```

### Running the Analysis
1. **Clone the repository:**
   ```bash
   git clone https://github.com/taroon23/Media-TV_MMM.git
   cd Media-TV_MMM
   ```

2. **Open the notebook:**
   ```bash
   jupyter notebook Case Study - Taroon Ganesh.ipynb
   ```

3. **Run all cells** (or follow the step-by-step sections)

The notebook is organized into clear sections:
1. Data Preparation & Validation
2. Feature Engineering
3. Model Training & Evaluation
4. Uncertainty Quantification (Bootstrap)
5. Channel ROI Analysis
6. Media Mix Optimization
7. Sensitivity Analysis
8. Executive Summary & Recommendations

---

## 🧪 What I Learned

### Technical Skills
- **Advanced feature engineering** for time-series marketing data (adstock, saturation curves)
- **Bootstrap resampling** for uncertainty quantification (200 iterations)
- **Time-series cross-validation** to prevent data leakage
- **Out-of-sample validation** for optimization (train on train, validate on test)
- **Ridge regression** with proper regularization tuning

### Domain Knowledge
- **Marketing Mix Modeling** fundamentals (diminishing returns, carryover, saturation)
- Media channel behavior (TV = reach, Digital = precision)
- Why ROI < 1.0 is common in mature campaigns (saturation)

### Business Thinking
- Translating model outputs → actionable recommendations
- Designing A/B test protocols for validation
- Balancing statistical rigor with business simplicity

---

## 🎓 Key Challenges & Solutions

### Challenge 1: Multicollinearity
**Problem:** Media channels are correlated (spend often moves together)
**Solution:** 
- Used Ridge regression (handles multicollinearity better than OLS)
- Focused on simplest model (Model A) for interpretation
- Validated with Bayesian model (different approach, same conclusions)

### Challenge 2: Distinguishing Short-term vs Long-term Effects
**Problem:** Does ad spend impact this week's viewership, or next week's?
**Solution:**
- Adstock transforms capture carryover (decay parameters)
- Tested multiple decay rates (0.5 for TV, 0.7 for Digital)
- Validated with Bayesian MMM that learns decay from data

### Challenge 3: Optimization Overfitting
**Problem:** Easy to find "optimal" allocation that only works on test data
**Solution:**
- Two-stage validation: optimize on train, validate on test
- Grid search over 225 allocation scenarios
- Sensitivity analysis confirms robustness (±20% budget scenarios)

---

## 📚 What's Next?

**Potential Enhancements:**
- [ ] Add genre-specific models (drama vs comedy vs reality)
- [ ] Test non-linear models (XGBoost, neural networks)
- [ ] Incorporate social media signals (Twitter mentions, sentiment)
- [ ] Build show-level models for top performers
- [ ] Add competitive spend data (where available)

**If I were doing this in production:**
- Automate weekly retraining pipeline
- Build Streamlit dashboard for real-time optimization
- Set up monitoring for coefficient drift
- A/B test recommendations before full rollout

---

## Feedback Welcome!

I'm always looking to improve my modeling and analysis. If you have suggestions on:
- Better validation approaches
- Alternative model specifications
- Clearer ways to communicate results
- Interesting extensions to try

---

## Connect - Taroon Ganesh (Data Science Grad - USC)

- **LinkedIn:** [linkedin.com/in/taroon-ganesh-27b83b171/](https://www.linkedin.com/in/taroon-ganesh-27b83b171/)
- **Email:** taroon2k@gmail.com

---
