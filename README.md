## 🧩 Project Pipeline  

 ```mermaid
flowchart TD
    A["Data Source (Dune API)"] --> B["Data Extraction (CSV Export)"]
    B --> C["Preprocessing & EDA (Jupyter Notebook)"]
    C --> D["Feature Engineering (Numerical + Categorical Variables)"]
    D --> E["Model Training (Logistic Regression)"]
    E --> F["Model Serialization (Pickle)"]
    F --> G["Deployment via Docker"]
    G --> H["Hosting on AWS Elastic Beanstalk"]

```

# Crypto-Trader-Classifier-good_vs_bad


### Problem Statement:

In decentralized trading ecosystems, founders, projects, and platforms often struggle to distinguish between profitable (good) and unprofitable (bad) traders.
Understanding trader quality is crucial for:

* Designing better incentive systems

* Driving sustainable trading activity

* Improving user retention and growth

* Supporting on-chain project strategy and analytics

The goal of this project is to build a machine learning model that can predict whether a trader is likely to be “good” (profitable, consistent, active) based on their on-chain activity features.


### 🧩 Data Source
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


### 🔍 Summary from Exploratory Data Analysis (EDA)
---

From the Jupyter Notebook (good_bad_trader_train.ipynb), several important behavioral insights emerged:

High Mutual Information:

trader_volume_status (≈ 0.362) and trader_weekly_frequency_status (≈ 0.334) are both highly informative predictors of trader quality.

Consistency Matters:

active_weeks shows moderate positive correlation (≈ 0.489) with trader quality.  Persistent activity across many weeks is a strong signal of good trading behavior.

Raw Volume ≠ Goodness:

total_volume shows almost no linear correlation (≈ 0.026) with being a good trader — suggesting the relationship is nonlinear (moderate traders can outperform extreme-volume ones).


### 🧠 Interpretation and Implications
---

| Stakeholder                 | Implication                                                                                                         |
| --------------------------- | ------------------------------------------------------------------------------------------------------------------- |
| **Projects / Protocols**    | Identify and reward “good” traders early; focus marketing spend and incentive structures on these high-value users. |
| **Founders / Growth Teams** | Design tiered reward systems (e.g., OG badges, VIP rewards) based on predicted good trader probability.             |
| **Traders / Users**         | Understand what behaviors correlate with sustained profitability and network influence.                             |
| **Analysts / Researchers**  | Gain empirical insight into the behavioral determinants of healthy market participation.                            |



### 🧱 Deployment Workflow
---

1. Model Serialization: The trained model (good_bad_trader_log_reg.bin) and DictVectorizer were serialized using Python’s pickle library for deployment.
2. API Service: A FastAPI service (predict_service.py) was built to serve real-time predictions via a REST API endpoint: POST /predict



### 🐳 Containerization
---

A lightweight Docker image was built using Python 3.12-slim and pipenv for environment management.

To build and run locally:
 docker build -t good_bad_trader .
 docker run -p 8000:8000 good_bad_trader

### ☁️ Deployment to AWS Elastic Beanstalk
The Docker image was deployed to AWS Elastic Beanstalk, leveraging a single-container environment for scalable serving.
Once deployed, predictions can be made via:
 python3 predict_test.py


💻 Tech Stack
---

* Data Source: Dune Analytics API

* Libraries: pandas, scikit-learn, matplotlib, seaborn, fastapi

* Virtual environment: pipenv

* Containerization: Docker

* Cloud deployment: AWS Elastic Beanstalk



# Conclusion
---

This project demonstrates an end-to-end ML workflow from on-chain data acquisition to deployed prediction API for identifying valuable traders in decentralized ecosystems.

By quantifying behavioral quality, we can move toward data-driven retention, fair incentives, and smarter ecosystem growth.

