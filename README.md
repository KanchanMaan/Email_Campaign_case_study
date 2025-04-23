# Email Campaign Optimization Case Study

A concise end‑to‑end Python pipeline to analyze, model, and optimize an email marketing campaign for improved click‑through rate (CTR).

## Table of Contents

- [HighLights](#Highlights)
- [Project Overview](#project-overview)  
- [Data Description](#data-description)  
- [Baseline Performance](#baseline-performance)  
- [Modeling Approach](#modeling-approach)  
- [Expected Improvement & Testing Plan](#expected-improvement--testing-plan)  
- [Segment Insights](#segment-insights)  

---

## Highlights
- **Baseline Metrics**: Open rate 10.35%, click‑through rate 2.12%.  
- **Predictive Model**: Random Forest with key features; projected CTR uplift ~17% at optimal threshold.  
- **A/B Testing**: Simulation, group assignment, and statistical validation framework.  
- **Segment Insights**: Impact of email length (long vs. short), personalization, and purchase history.

---

## Project Overview

We were tasked to analyze the performance of a recent email campaign, build a model to predict who is most likely to click the link inside an email, and quantify the expected lift in CTR.  
Key questions answered:

1. **What percentage of users opened the email and clicked the link?**  
2. **Can we build a model to optimize future sends for maximum click probability?**  
3. **By how much would the model improve CTR, and how would we test it?**  
4. **What interesting patterns emerge for different user segments?**

---

## Data Description

- **email_table** (100 000 rows):  
  - `email_text`: “long” vs. “short” copy  
  - `email_version`: “personalized” (name included) vs. “generic”  
  - `hour`, `weekday`, `user_country`, `user_past_purchases`  
- **email_opened_table**: 10 345 unique `email_id` opened at least once  
- **link_clicked_table**: 2 119 unique `email_id` clicked at least once  

---

## Baseline Performance

- **Emails sent:** 100 000  
- **Emails opened:** 10 345 → **Open Rate** = 10.35%  
- **Links clicked:** 2 119 → **Click Rate** = 2.12%  
- **Click-through rate (clicks / opens):** 20.48%

> **Q1 Answer:** 10.35% of recipients opened the email and 2.12% clicked the link, yielding a 20.48% CTR among opens.

---

## Modeling Approach

1. **Feature Engineering**  
   - One-hot encode: `email_text`, `email_version`, `weekday`, `hour`, `user_country`, `purchase_bin` (quantile‐based on `user_past_purchases`)  
   - Interaction term: `personalized_x_high` = 1 if “personalized” **and** in top purchase-bin  

2. **Train/Test Split**  
   - 80/20 stratified by click label for unbiased evaluation  

3. **Algorithms & Tuning**  
   - **Logistic Regression** (`C` grid)  
   - **Random Forest** (`n_estimators`, `max_depth` grid)  
   - 5-fold CV optimizing ROC AUC  
   - Calibration with Platt scaling  

> **Q2 Answer:** We trained both Logistic Regression and Random Forest models. The Random Forest achieved the best test ROC AUC (0.75) and is our recommended model for deployment.

---

## Expected Improvement & Testing Plan

- **Simulation:** Vary classification threshold _t_ to assign “model” vs. “random” sends.  
- **Optimal threshold (_t_=0.25):**  
  - Model group size ≈ 40 000, predicted CTR ≈ 24%  
  - Random control expected CTR ≈ 20.5%  
  - **Relative lift ≈ 17%**  

> **Q3 Answer:** We expect a ~17% relative uplift in CTR. To validate, we’d run a classic A/B test—split users into model‐recommended vs. random groups, measure CTR over 1–2 weeks, and confirm significance with a two-sample z-test (α=0.05).

---

## Segment Insights

| Segment                 | Model CTR | Baseline CTR | Lift (pp) |
|-------------------------|-----------|--------------|-----------|
| High spenders (top bin) | 30%       | 25%          | +5        |
| New users (bottom bin)  | 15%       | 12%          | +3        |
| Short vs. Long text     | 22% vs. 19% | 19%        | +3        |
| Weekend sends           | 27%       | 20%          | +7        |
| Evening (18–21h)        | 26%       | 20%          | +6        |

> **Q4 Answer:**  
> - **Personalization** yields +5 pp lift for high-value purchasers.  
> - **Short emails** are +3 pp more effective for new users.  
> - **Weekend evening** sends boost CTR by +6–7 pp.

---
