# Media-TV_MMM

This project implements an **end-to-end Marketing Mix Model (MMM)** using both **Ridge Regression** and a **Bayesian Hill-Adstock model** to quantify channel effectiveness, measure ROI, and simulate the impact of alternative media strategies. The notebook includes full data engineering, feature creation, modeling, diagnostics, and spend optimization.

---

## Project Overview

This repository contains a full MMM pipeline built from raw episodic marketing data, designed to answer key business questions such as:

- Which marketing channels are most effective in driving the target KPI (e.g., viewership, conversions, or revenue)?  
- How do diminishing returns (saturation) and carryover effects (adstock) shape channel performance?  
- What happens to performance metrics if a channel’s spend is increased or decreased?  
- How should budgets be allocated across channels to maximize overall impact?

The project includes multiple modeling approaches to ensure robustness and interpretability.

---

## Key Components

### **1. Data Cleaning & Gold Layer Preparation**
- Consolidated and refined raw episodic data into a **Gold dataset** suitable for modeling  
- Generated control variables and dummy-encoded categorical factors  
- Added lag features and time indices to account for temporal dependencies  

### **2. Advanced Feature Engineering**
- Applied **geometric adstock** transformations per channel  
- Implemented **Hill saturation** to model diminishing returns  
- Calculated share-of-voice, total spend, episode-type flags (Premiere, Finale, etc.)  
- Included series-level and season-level fixed effects

### **3. Ridge Regression MMM (Linear & Nonlinear Feature Sets)**
Built three Ridge models with:
- **Log media**  
- **Log media + adstock**  
- **Hill-saturation transforms**

Each model was evaluated using out-of-time R² and RMSE.

### **4. Bayesian Hill-Adstock MMM**
A probabilistic model incorporating:
- Non-linear saturation  
- Long-tail carryover effects  
- Parameter uncertainty (credible intervals)  

This improves interpretability and robustness in high-multicollinearity settings.

### **5. Scenario Simulations**
Simulated the impact of alternative media strategies, such as:
- +20% increase in a specific channel  
- Rebalancing spend across categories  
- Evaluating lift in the chosen KPI  

### **6. Budget Optimization**
Performed a constrained grid search (±10% spend bands) to identify:
- Revenue-maximizing allocation  
- Most efficient channel mix  
- Relative ROI and marginal contributions  

---

## Performance Summary

Across multiple Ridge and Bayesian specifications, the best model achieved:

- **Out-of-time R² ≈ 0.96**  
- Strong generalization under time-based validation  
- Stable and interpretable channel coefficients  
- Consistent model behavior across linear and Bayesian frameworks  

---

This project demonstrates:
- Full lifecycle MMM development  
- Engineering adstock & saturation features  
- Linear vs Bayesian modeling tradeoffs  
- Interpreting media ROI  
- Running simulations & optimizations  
- Managing multicollinearity in marketing data  

---

## 📬 Contact

**Taroon Ganesh**  
Data Science Graduate @ USC  
*https://www.linkedin.com/in/taroon-ganesh-27b83b171/*

---
