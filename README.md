# 🏠 Predicting Occupancy Efficiency in Seattle Airbnb Listings

> **Course Project — Missouri University of Science and Technology**
> Team: Krypton Squad | Aug 2025 – Dec 2025

---

## 👥 Team Members

| Name | GitHub |
|------|--------|
| Jhansimala Silla | [@sillajhansimala](https://github.com/sillajhansimala) |
| Vishesha Bitla | [@vishesha-bitla](https://github.com/bitlavishesha1) |
| Gafaruddin Shaik | [@gafaruddin-shaik](https://github.com/Gafarshaik78) |
| Keerthi Sri Cherukuri | [@keerthisri-cherukuri](https://github.com/keerthisri-cherukuri) |


---

## 📌 Research Question

> **Which variables most strongly influence the 30-day occupancy of Airbnb listings, and which algorithm best predicts high-occupancy outcomes?**

---

## 🎯 Project Overview

Most Airbnb analytics research focuses on price or yearly revenue forecasting. This project takes a different approach — we focused on **30-day occupancy efficiency** as a more actionable metric for hosts. High occupancy is a prerequisite for high revenue, and predicting it enables tactical adjustments in near-real-time.

### Who benefits?
**Airbnb hosts in Seattle** — this model helps them understand:
- What price range maximizes bookings
- How guest reviews directly impact occupancy
- How availability windows affect demand
- What makes a listing land in the top 25% of performers

---

## 📂 Repository Contents

| File | Description |
|------|-------------|
| `seattle_airbnb_code.ipynb` | Full pipeline: cleaning, EDA, feature engineering, modeling |
| `Seattle_new.xlsx` | Seattle Airbnb dataset (sourced from Inside Airbnb) |
| `Krypton_Squad_Research_Paper.pdf` | Published research paper |
| `Krypton_Squad_Final_Presentation.pptx` | Final presentation (includes narration) |

---

## ⚠️ Key Challenge We Solved

The original Seattle dataset included `estimated_occupancy_365` (yearly), but our project scope was **30-day occupancy**. We discovered this mismatch mid-project, which would have caused severely inaccurate precision and recall values.

**Our fix — we derived our own 30-day variables using:**

```python
estimated_occupancy_30 = 30 - availability_30
estimated_revenue_30   = price × estimated_occupancy_30
occupancy_rate_30      = estimated_occupancy_30 / 30   # target variable (0–1)
```

This correction was critical to the validity of all model results.

---

## 🛠️ Pipeline Summary

### 1. Data Cleaning & Feature Engineering
- Cleaned price, response rate, and boolean fields
- Imputed missing values using median/mode
- Clipped outliers at 1st–99th percentiles
- Log-transformed skewed features (`price`, `reviews`, `revenue`)
- Engineered `dist_km_downtown` using Haversine formula
- Reduced multicollinearity via VIF (iterative feature elimination)

### 2. Target Variable
Continuous `occupancy_rate_30` was converted to a **binary classification target**:
- **Class 1 (Very High Occupancy):** Top quartile (Q4) — top 25%
- **Class 0 (Not Very High):** Bottom 75% (Q1–Q3)

### 3. Models Trained
| Model | Type |
|-------|------|
| Linear Regression | Continuous baseline |
| Logistic Regression | Binary + threshold tuning (0.1–0.9) |
| Random Forest | Ensemble classifier |
| Decision Tree | Interpretable classifier ✅ Best model |
| Naive Bayes | Probabilistic benchmark |

---

## 📊 Model Results (cutoff = 0.4)

| Model | Accuracy | Precision | Recall | F1-Score |
|-------|----------|-----------|--------|----------|
| Logistic Regression | 0.9213 | 0.8271 | 0.8567 | 0.8416 |
| Random Forest | 0.9410 | 0.8649 | 0.8985 | 0.8814 |
| **Decision Tree** | **0.9476** | **0.8856** | **0.9015** | **0.8935** |
| Naive Bayes | 0.4006 | 0.2818 | 0.9403 | 0.4336 |

### Why we focused on Recall & Precision
We used **Youden's J statistic** to identify the optimal threshold (0.4), balancing recall (catching true high-occupancy listings) and precision (minimizing false positives).

---

## 🌳 Best Model: Decision Tree

The Decision Tree was selected for its **combination of performance and interpretability**:

- Primary split: `log_estimated_revenue_30 ≤ 7.585`
- Secondary split: `price ≤ 147.5`
- Highest F1-score (0.8935) among all models
- Human-readable rules — hosts can act on insights directly

---

## ⭐ Do Reviews Affect Occupancy?

**Yes — significantly.**

| Review Count | Occupancy Trend |
|---|---|
| 0–10 reviews | Low & unstable |
| 11–50 reviews | Noticeable improvement |
| 50–200 reviews | Strong occupancy trend |
| 200+ reviews | Highest booking consistency |

Key insight: **review volume matters more than review length or rating alone.** Social proof drives bookings.

---

## 💡 Actionable Insights for Hosts

1. **Price moderately** — listings priced below ~$147.50 with solid revenue signals have the highest probability of Very High Occupancy
2. **Collect reviews actively** — review volume is the strongest non-revenue predictor
3. **Optimize availability** — high availability_30 negatively correlates with occupancy; strategic availability windows signal demand

---

## 🧠 SHAP Analysis

SHAP (Shapley Additive Explanations) values were computed on the Decision Tree to explain feature contributions:

1. `log_estimated_revenue_30` — strongest positive influence
2. `price` — nonlinear; moderate pricing improves scores, high pricing reduces them
3. `number_of_reviews` — significant positive feature
4. `availability_30` / `availability_365` — negative SHAP influence

---

## 🚀 How to Run

```bash
# Clone the repository
git clone https://github.com/sillajhansimala/seattle-airbnb-occupancy.git
cd seattle-airbnb-occupancy

# Install dependencies
pip install pandas numpy matplotlib scikit-learn statsmodels shap openpyxl

# Open the notebook
jupyter notebook seattle_airbnb_code.ipynb
```

---

## 🔮 Future Work

- Seasonal trend integration
- Dynamic pricing models
- Sentiment analysis on review text
- SHAP-based personalized host recommendations

---

## 📚 References

- Wang, D., & Nicolau, J. L. (2017). Price determinants of sharing economy accommodation. *International Journal of Hospitality Management*, 62, 120–131.
- Zhang, Z., & Wei, L. (2019). Predicting short-term rental prices using machine learning. *Journal of Information and Data Management*, 10(3), 200–214.
- Inside Airbnb: [insideairbnb.com](http://insideairbnb.com)

---

## 🙏 Acknowledgement

Special thanks to **Prof. Minsek "Cal" Ko, PhD** — Assistant Professor, Department of Business & Information Technology, Missouri S&T — for guiding our team from the very beginning to the final presentation. His mentorship and feedback were invaluable throughout this project.

---

> 📍 Missouri University of Science and Technology | Information Science | Aug–Dec 2025
> Team: Jhansimala Silla, Vishesha Bitla, Keerthi Sri Cherukuri, Gafaruddin Shaik
