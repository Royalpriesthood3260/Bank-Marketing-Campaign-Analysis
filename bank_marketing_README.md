# 🏦 Bank Marketing Campaign Analysis

A data analysis project in Python on a Portuguese bank's telemarketing campaign data, covering data cleaning and 5 visualizations to identify which customers are most likely to subscribe to a term deposit product.

---

## 🎯 Objective

Banks run telemarketing campaigns to promote financial products like term deposits. Understanding which customers are most likely to subscribe has direct business value — it reduces wasted outreach, improves campaign ROI, and helps prioritize the right segments.

The goal of this project is to:

- **Clean and prepare** 11,162 customer records before analysis
- Build **5 visualizations** to explore subscription patterns
- Identify the **strongest predictors** of customer subscription
- Provide **actionable recommendations** for future campaigns

---

## 📁 Project Structure

```
bank-marketing-campaign/
│
├── data/
│   └── bank_csv.xls
│
├── scripts/
│   └── bank_marketing_analysis.py
│
├── plots/
│   ├── plot1_subscription_by_job.png
│   ├── plot2_subscription_by_month.png
│   ├── plot3_balance_distribution.png
│   ├── plot4_previous_outcome.png
│   └── plot5_age_distribution.png
│
└── README.md
```

---

## 📊 Dataset

| Field | Description |
|-------|-------------|
| `age` | Customer age |
| `job` | Type of job |
| `marital` | Marital status |
| `education` | Education level |
| `balance` | Average yearly account balance (EUR) |
| `contact` | Contact communication type |
| `campaign` | Number of contacts during this campaign |
| `poutcome` | Outcome of previous marketing campaign |
| `deposit` | Target — did the client subscribe? (yes/no) |

**11,162 rows · 17 columns · No missing values after cleaning**

---

## 🧹 Data Cleaning

- Replaced `"unknown"` values with `NaN` in job, education, contact, poutcome
- Converted `pdays = -1` (never previously contacted) to `NaN`
- Capped campaign outliers at 20 contacts per client
- Clipped balance outliers beyond ±3 standard deviations
- Encoded target variable `deposit` as binary `subscribed` (1 = yes, 0 = no)

---

## 📈 Visualizations

| Plot | Description |
|------|-------------|
| Plot 1 | Subscription rate by job type |
| Plot 2 | Subscription rate by month |
| Plot 3 | Account balance distribution by outcome |
| Plot 4 | Previous campaign outcome impact |
| Plot 5 | Age distribution by outcome |

---

## 💡 Key Findings

- **47.4%** overall subscription rate — a well-balanced dataset
- **Students (74.7%)** and **retirees (66.3%)** are the highest converting job segments
- **March (89.9%), December (90.9%), September (84.3%)** are the best campaign months
- **May** is the worst month at only **32.8%** despite likely being the busiest
- Subscribers hold a significantly higher avg balance (**$1,657 vs $1,171**)
- **Longer call duration** strongly associates with subscription — quality conversations matter more than volume of calls
- Customers who previously subscribed convert at **91.3%** — the single strongest predictor
- **Age is NOT a useful predictor** — job type and balance matter far more

---

## 🎯 Recommendations

- **Prioritize students and retirees** — they convert at 74.7% and 66.3%, far above the 47.4% average
- **Run campaigns in March, September and December** — these months yield 84–91% conversion rates; avoid May (32.8%)
- **Re-target previous subscribers first** — they convert at 91.3% and require the least effort
- **Focus outreach on higher balance customers** — subscribers average $1,657 vs $1,171 for non-subscribers
- **Prioritize call quality over volume** — longer call duration is the strongest indicator of subscription

---

## 🚀 How to Run

**1. Clone the repo**
```bash
git clone https://github.com/yourusername/bank-marketing-campaign.git
```

**2. Install dependencies**
```bash
pip install pandas matplotlib
```

**3. Run the script**
```bash
python scripts/bank_marketing_analysis.py
```

Plots will save automatically to the `plots/` folder.

---

## 📦 Dependencies

| Package | Purpose |
|---------|---------|
| `pandas` | Data wrangling |
| `matplotlib` | Visualizations |

---

## Dataset Source
[Bank Marketing Dataset — Kaggle](https://www.kaggle.com/datasets/janiobachmann/bank-marketing-dataset)

---

## 🙋 Author
Made by **[Your Name]** · [LinkedIn](https://linkedin.com/in/yourprofile) · [GitHub](https://github.com/yourusername)
