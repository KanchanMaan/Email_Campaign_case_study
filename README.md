# Email Campaign Optimization Case Study

A concise end‑to‑end Python pipeline to analyze, model, and optimize an email marketing campaign for improved click‑through rate (CTR).

## Highlights
- **Baseline Metrics**: Open rate 10.35%, click‑through rate 2.12%.  
- **Predictive Model**: Random Forest with key features; projected CTR uplift ~17% at optimal threshold.  
- **A/B Testing**: Simulation, group assignment, and statistical validation framework.  
- **Segment Insights**: Impact of email length (long vs. short), personalization, and purchase history.

## Quick Start
```bash
git clone https://github.com/your_org/email-campaign-optimization.git
cd email-campaign-optimization
pip install -r requirements.txt

# Compute baseline metrics
python src/data_loading.py

# Train & evaluate models
python src/modeling.py

# Simulate A/B test & measure uplift
python src/ab_test_simulation.py
