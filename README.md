# Financial Regression Discontinuity Design (RDD): Russell 1000/2000 Index Inclusion

## Overview

This repository contains a Python implementation of an enhanced **Regression Discontinuity Design (RDD)** to analyze the causal effects of index reconstitution. Specifically, it models the threshold between the Russell 1000 (large-cap) and Russell 2000 (small-cap) indices.

While basic econometrics often uses a **Sharp RDD** for this analysis, real-world financial market frictions make a sharp design inadequate. This code implements a **Fuzzy RDD** using an Instrumental Variable (IV) approach (Two-Stage Least Squares) to account for index banding rules, and extends the outcome variables to include both Cumulative Abnormal Returns (CAR) and Abnormal Trading Volume.

## 🚀 Key Financial Enhancements

The standard RDD logic assumes a hard cutoff at rank 1000. This code upgrades that logic with three critical financial realities:

1. **Russell Banding Rules (Fuzzy RDD Framework):** Since 2007, FTSE Russell has utilized a "banding" rule to prevent excessive index turnover. A stock near the cutoff doesn't automatically switch indices; it must cross a cumulative market cap tolerance band. Therefore, actual inclusion ($D_i$) is partially determined by lagged index membership. The script models the rank threshold as an instrumental variable ($Z_i$) for actual inclusion.
2. **Dual Outcomes (Volume vs. Price):** Index inclusion effects are heavily driven by mechanical buying from passive funds. The discontinuity is often much sharper and more persistent in **Abnormal Trading Volume** than in prices (CAR), as active managers quickly arbitrage price discrepancies. This code evaluates both.
3. **Robust Standard Errors:** Returns of stocks near the threshold often suffer from heteroskedasticity and clustered volatility. The 2SLS implementation utilizes `HC1` robust standard errors.

## 🧮 Core Econometric Formulas

To formally estimate the Fuzzy RDD and correct for the banding tolerance, the code relies on the following Two-Stage Least Squares (2SLS) framework within an optimal local bandwidth:

**1. Potential Outcome Data Generation Process (DGP):**
$$Y_i = f(X_i) + \tau D_i + \epsilon_i$$
Where $f(X_i)$ is the smooth fundamental effect of the running variable (market cap rank $X_i$), $D_i$ is actual index inclusion, and $\epsilon_i$ represents idiosyncratic noise.

**2. First-Stage Regression (Instrumenting Actual Inclusion):**
$$D_i = \gamma_0 + \gamma_1 Z_i + \gamma_2 X_i + \gamma_3(X_i \cdot Z_i) + \nu_i$$
Where $Z_i$ is the theoretical threshold instrument ($Z_i=1$ if $X_i \ge c$). $\gamma_1$ captures the jump in the probability of actual inclusion at the cutoff.

**3. Second-Stage Regression (Estimating the Causal Effect):**
$$Y_i = \beta_0 + \beta_1 \hat{D}_i + \beta_2 X_i + \beta_3(X_i \cdot Z_i) + u_i$$
Where $\hat{D}_i$ is the predicted inclusion from the first stage. The coefficient $\beta_1$ isolates the causal Local Average Treatment Effect (LATE).
