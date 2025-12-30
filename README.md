# Proactive Financial Fraud Detection: A Machine Learning Approach

## 📌 Project Overview
This project involves developing a machine learning model to proactively detect fraudulent transactions in a mobile money transfer simulation (PaySim dataset). The goal is to identify fraudulent behavior patterns, such as "account emptying" or "system bypassing," and provide actionable insights for infrastructure updates to prevent future financial loss.

The solution utilizes a **Random Forest Classifier** optimized for highly imbalanced data, achieving robust detection rates while minimizing friction for legitimate customers.

---

## 🚀 Key Features
* **End-to-End Workflow:** From data cleaning and outlier handling to model deployment and evaluation.
* **Advanced Feature Engineering:** Created behavioral features like `errorBalanceOrig` (identifying mathematical discrepancies in transactions) which proved to be strong fraud predictors.
* **Imbalance Handling:** Utilized `class_weight='balanced'` to effectively handle the <0.2% fraud rate without synthetic oversampling.
* **Robust Evaluation:** Focused on Precision-Recall (PR-AUC) and False Positive Rates rather than simple accuracy.
* **Business Intelligence:** Translates model outputs into concrete infrastructure recommendations (e.g., Velocity Checks, Real-Time Logic Blocks).

---

## 🛠️ Tech Stack
* **Language:** Python 3.x
* **Libraries:**
    * `pandas`, `numpy` (Data Manipulation)
    * `matplotlib`, `seaborn` (Visualization)
    * `scikit-learn` (Machine Learning: Random Forest, Metrics)
    * `statsmodels` (Multicollinearity/VIF Analysis)

---

## 📂 Dataset
The project uses the **PaySim** dataset, a synthetic dataset that simulates mobile money transactions.
* **Original Size:** ~6.3 million rows.
* **Filtering:** The analysis is restricted to `TRANSFER` and `CASH_OUT` transaction types, as 100% of fraud occurs within these modes.
* **Target Variable:** `isFraud` (0 = Legitimate, 1 = Fraudulent).

---

## 📊 Methodology

### 1. Data Cleaning & Pre-processing
* **Missing Values:** Handled via median imputation (robust to skew).
* **Outliers:** Applied **Winsorization (Capping)** at the 99th percentile to restrain extreme values while preserving "high-value" fraud signals.
* **Multicollinearity:** Checked using Variance Inflation Factor (VIF) to ensure feature independence.

### 2. Feature Selection
* **Dropped:** `nameOrig`, `nameDest` (High cardinality IDs), `step` (Simulation time).
* **Created:**
    * `errorBalanceOrig`: Captures if `NewBalance != OldBalance - Amount`.
    * `errorBalanceDest`: Captures discrepancies in the destination account.

### 3. Model Training
* **Algorithm:** Random Forest Classifier (`n_estimators=100`, `max_depth=12`).
* **Strategy:** Used `class_weight='balanced'` to penalize the model heavily for missing fraud cases.
* **Split:** 80% Training / 20% Testing (Stratified).

---

## 📈 Results & Performance

| Metric | Score | Description |
| :--- | :--- | :--- |
| **PR-AUC** | **0.85+** | (High precision-recall balance, crucial for fraud) |
| **Recall** | **~0.78** | (Captures majority of fraud cases) |
| **Precision** | **~0.95** | (Low false alarm rate) |

### Key Predictors (Feature Importance)
1.  **`oldbalanceOrg`**: Initial account balance (Fraudsters target high-liquidity accounts).
2.  **`amount`**: Transaction size (Attempts to drain funds in single go).
3.  **`errorBalanceOrig`**: Mathematical mismatch in balance updates.

---

## 🛡️ Strategic Recommendations (Action Plan)

Based on the model insights, the following infrastructure updates are recommended:

1.  **"Golden Rule" Validation (Real-Time):**
    * *Action:* Immediately block any transaction where `OldBalance - Amount != NewBalance`.
    * *Why:* The model identified `errorBalanceOrig` as a top predictor of fraud.

2.  **Velocity Limits:**
    * *Action:* Trigger 2FA if >90% of an account's funds are transferred within 1 hour.
    * *Why:* Prevents rapid account draining (`oldbalanceOrg` predictor).

3.  **Mule Account Monitoring:**
    * *Action:* Flag accounts that receive a `TRANSFER` and perform a `CASH_OUT` within <10 minutes.

---

## ⚙️ How to Run

1.  **Clone the repository:**
    ```bash
    git clone [https://github.com/yourusername/fraud-detection-project.git](https://github.com/yourusername/fraud-detection-project.git)
    ```
2.  **Install dependencies:**
    ```bash
    pip install pandas numpy matplotlib seaborn scikit-learn statsmodels
    ```
3.  **Run the Notebook:**
    Open `main.ipynb` in Jupyter Notebook or VS Code and run all cells.

---

## 👤 Author
* **Name:** [Your Name]
* **Contact:** [Your Email / LinkedIn]

---
*Disclaimer: This project is for educational purposes using synthetic data.*
