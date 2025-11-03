## 🧩 Project Pipeline  

```mermaid
flowchart LR
    A[Dune Analytics API] -->|Export CSV| B["Data Preprocessing & EDA (Jupyter Notebook)"]
    B --> C[Model Training (Logistic Regression)]
    C --> D[Model Serialization (good_bad_trader_log_reg.bin)]
    D --> E[FastAPI Service (predict_service.py)]
    E --> F[Containerization (Dockerfile + Pipenv)]
    F --> G[AWS Elastic Beanstalk Deployment]

    style A fill:#fdf5e6,stroke:#d4a017,stroke-width:2px
    style B fill:#e6f3ff,stroke:#0077cc,stroke-width:2px
    style C fill:#e8ffe6,stroke:#33aa33,stroke-width:2px
    style D fill:#fff6e6,stroke:#ffb733,stroke-width:2px
    style E fill:#f0e6ff,stroke:#7b68ee,stroke-width:2px
    style F fill:#fff0f5,stroke:#cc3366,stroke-width:2px
    style G fill:#e6fff9,stroke:#00b386,stroke-width:2px
```

# Crypto-Trader-Classifier-good_vs_bad

##### A Machine Learning Pipeline for Understanding Trader Quality and Engagement Behavior Using Dune Analytics Data

Problem Statement:

In decentralized trading ecosystems, founders, projects, and platforms often struggle to distinguish between profitable (good) and unprofitable (bad) traders.
Understanding trader quality is crucial for:

* Designing better incentive systems

* Driving sustainable trading activity

* Improving user retention and growth

* Supporting on-chain project strategy and analytics

The goal of this project is to build a machine learning model that can predict whether a trader is likely to be “good” (profitable, consistent, active) based on their on-chain activity features.


🧩 Data Source
---
The dataset originates from Dune Analytics, queried through its API and exported to CSV for offline analysis.
Each row represents an individual wallet address, with associated behavioral features that describe activity, engagement, and trading frequency.
| Feature Type | Column                           | Description                                       |
| ------------ | -------------------------------- | ------------------------------------------------- |
| Numerical    | `active_weeks`                   | Number of distinct weeks with activity            |
| Numerical    | `total_volume`                   | Total trading volume (USD)                        |
| Numerical    | `tx_count_365d`                  | Total transactions in the past year               |
| Categorical  | `trader_activity_status`         | Activity-level bucket (e.g., Low, Middle, High)   |
| Categorical  | `trader_weekly_frequency_status` | Frequency bucket (e.g., Newbie, Regular, OG)      |
| Target       | `good_trader`                    | Binary indicator: 1 = good trader, 0 = bad trader |


🔍 Summary from Exploratory Data Analysis (EDA)

From the Jupyter Notebook (good_bad_trader_train.ipynb), several important behavioral insights emerged:

High Mutual Information:

trader_volume_status (≈ 0.362) and

trader_weekly_frequency_status (≈ 0.334)
are both highly informative predictors of trader quality.

Consistency Matters:

active_weeks shows moderate positive correlation (≈ 0.489) with trader quality.
Persistent activity across many weeks is a strong signal of good trading behavior.

Raw Volume ≠ Goodness:

total_volume shows almost no linear correlation (≈ 0.026) with being a good trader — suggesting the relationship is nonlinear (moderate traders can outperform extreme-volume ones).

Interpretation and Implications

| Stakeholder                 | Implication                                                                                                         |
| --------------------------- | ------------------------------------------------------------------------------------------------------------------- |
| **Projects / Protocols**    | Identify and reward “good” traders early; focus marketing spend and incentive structures on these high-value users. |
| **Founders / Growth Teams** | Design tiered reward systems (e.g., OG badges, VIP rewards) based on predicted good trader probability.             |
| **Traders / Users**         | Understand what behaviors correlate with sustained profitability and network influence.                             |
| **Analysts / Researchers**  | Gain empirical insight into the behavioral determinants of healthy market participation.                            |




