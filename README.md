# Financial Regression Discontinuity Design (RDD): Russell 1000/2000 Index Inclusion

## Overview

This repository contains a Python implementation of an enhanced **Regression Discontinuity Design (RDD)** to analyze the causal effects of index reconstitution. Specifically, it models the threshold between the Russell 1000 (large-cap) and Russell 2000 (small-cap) indices.

While basic econometrics often uses a **Sharp RDD** for this analysis, real-world financial market frictions make a sharp design inadequate. This code implements a **Fuzzy RDD** using an Instrumental Variable (IV) approach (Two-Stage Least Squares) to account for index banding rules, and extends the outcome variables to include both Cumulative Abnormal Returns (CAR) and Abnormal Trading Volume.

## 🚀 Key Financial Enhancements

The standard RDD logic assumes a hard cutoff at rank 1000. This code upgrades that logic with three critical financial realities:

1. **Russell Banding Rules (Fuzzy RDD Framework):** Since 2007, FTSE Russell has utilized a "banding" rule to prevent excessive index turnover. A stock near the cutoff doesn't automatically switch indices; it must cross a cumulative market cap tolerance band. Therefore, actual inclusion ($D_i$) is partially determined by lagged index membership. The script models the rank threshold as an instrumental variable ($Z_i$) for actual inclusion.
2. **Dual Outcomes (Volume vs. Price):** Index inclusion effects are heavily driven by mechanical buying from passive funds. The discontinuity is often much sharper and more persistent in **Abnormal Trading Volume** than in prices (CAR), as active managers quickly arbitrage price discrepancies. This code evaluates both.
3. **Robust Standard Errors:** Returns of stocks near the threshold often suffer from heteroskedasticity and clustered volatility. The 2SLS implementation utilizes `HC1` robust standard errors.
