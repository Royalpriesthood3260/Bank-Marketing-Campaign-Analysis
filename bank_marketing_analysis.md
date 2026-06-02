# 🏦 Bank Marketing Campaign Analysis — Python

## Libraries

```python
import pandas as pd
import matplotlib.pyplot as plt
import warnings
warnings.filterwarnings("ignore")
```

---

## 1. Load Data

```python
df = pd.read_csv("data/bank_csv.xls")
print(df.shape)   # (11162, 17)
print(df.head())
```

---

## 2. Data Cleaning

```python
# Replace 'unknown' strings with NaN
unknown_cols = ['job', 'education', 'contact', 'poutcome']
for col in unknown_cols:
    df[col] = df[col].replace('unknown', pd.NA)

# pdays = -1 means never previously contacted
df['pdays'] = df['pdays'].replace(-1, pd.NA)

# Cap campaign outliers at 20 (42 extreme rows)
df['campaign'] = df['campaign'].clip(upper=20)

# Clip balance outliers beyond ±3 standard deviations (173 rows)
mean_b, std_b = df['balance'].mean(), df['balance'].std()
df['balance'] = df['balance'].clip(lower=mean_b - 3*std_b, upper=mean_b + 3*std_b)

# Encode target as binary
df['subscribed'] = (df['deposit'] == 'yes').astype(int)

# Overall subscription rate
sub_rate = df['subscribed'].mean() * 100
print(f"Subscription rate: {sub_rate:.1f}%")  # 47.4%
```

---

## 3. Colors & Style

```python
plt.style.use('dark_background')

ACCENT = "#00C9A7"  # teal  — above average
WARN   = "#FF6B6B"  # red   — reference line
BLUE   = "#2E4A6A"  # navy  — below average bars
```

---

## 4. Plot 1 — Subscription Rate by Job

```python
job_rate = (df.dropna(subset=['job'])
              .groupby('job')['subscribed']
              .mean()
              .sort_values() * 100)

colors = [ACCENT if v >= sub_rate else BLUE for v in job_rate]

fig, ax = plt.subplots(figsize=(9, 6))
ax.barh(job_rate.index, job_rate.values, color=colors)
ax.axvline(sub_rate, color=WARN, linestyle='--', linewidth=1.5,
           label=f'Avg {sub_rate:.1f}%')
ax.set_title("Subscription Rate by Job", fontsize=14, fontweight='bold', pad=12)
ax.set_xlabel("Subscription Rate (%)")
ax.legend()
plt.tight_layout()
plt.savefig("plots/plot1_subscription_by_job.png", dpi=150, bbox_inches='tight')
plt.show()

# INSIGHT: Students (74.7%) and retirees (66.3%) are the highest converting segments.
# Blue-collar (36.4%) and entrepreneurs (37.5%) are the hardest to convert.
# Target students and retirees first in future campaigns.
```

---

## 5. Plot 2 — Subscription Rate by Month

```python
month_order = ['jan','feb','mar','apr','may','jun',
               'jul','aug','sep','oct','nov','dec']
month_rate = df.groupby('month')['subscribed'].mean() * 100
month_rate = month_rate.reindex([m for m in month_order if m in month_rate.index])

colors2 = [ACCENT if v >= sub_rate else BLUE for v in month_rate]

fig, ax = plt.subplots(figsize=(9, 6))
ax.bar(month_rate.index, month_rate.values, color=colors2)
ax.axhline(sub_rate, color=WARN, linestyle='--', linewidth=1.5,
           label=f'Avg {sub_rate:.1f}%')
ax.set_title("Subscription Rate by Month", fontsize=14, fontweight='bold', pad=12)
ax.set_ylabel("Subscription Rate (%)")
ax.set_ylim(0, 100)
ax.legend()
plt.xticks(rotation=45)
plt.tight_layout()
plt.savefig("plots/plot2_subscription_by_month.png", dpi=150, bbox_inches='tight')
plt.show()

# INSIGHT: March (89.9%), December (90.9%), September (84.3%) are peak months.
# May is worst at 32.8% despite being the busiest campaign period.
# Concentrate campaigns in Q4 and March for maximum conversion.
```

---

## 6. Plot 3 — Balance Distribution by Outcome

```python
fig, ax = plt.subplots(figsize=(9, 6))
ax.hist(df[df['subscribed']==1]['balance'], bins=40, alpha=0.7,
        color=ACCENT, label='Subscribed')
ax.hist(df[df['subscribed']==0]['balance'], bins=40, alpha=0.7,
        color=WARN, label='Not Subscribed')
ax.set_title("Balance Distribution by Outcome", fontsize=14, fontweight='bold', pad=12)
ax.set_xlabel("Account Balance ($)")
ax.set_ylabel("Count")
ax.legend()
plt.tight_layout()
plt.savefig("plots/plot3_balance_distribution.png", dpi=150, bbox_inches='tight')
plt.show()

# INSIGHT: Subscribers have a higher mean balance ($1,657 vs $1,171)
# and median balance ($733 vs $414).
# 42.5% of subscribers have a balance > $1,000 vs 30.5% of non-subscribers.
# Wealthier customers are more likely to invest — target higher balance segments.
```

---

## 7. Plot 4 — Previous Campaign Outcome

```python
pout_rate = (df.dropna(subset=['poutcome'])
               .groupby('poutcome')['subscribed']
               .mean()
               .sort_values(ascending=False) * 100)

colors4 = [ACCENT if v >= sub_rate else BLUE for v in pout_rate]

fig, ax = plt.subplots(figsize=(9, 6))
ax.bar(pout_rate.index, pout_rate.values, color=colors4)
ax.axhline(sub_rate, color=WARN, linestyle='--', linewidth=1.5,
           label=f'Avg {sub_rate:.1f}%')
ax.set_title("Subscription Rate by Previous Campaign Outcome",
             fontsize=14, fontweight='bold', pad=12)
ax.set_ylabel("Subscription Rate (%)")
ax.set_ylim(0, 100)
ax.legend()
plt.tight_layout()
plt.savefig("plots/plot4_previous_outcome.png", dpi=150, bbox_inches='tight')
plt.show()

# INSIGHT: Customers who previously subscribed convert at 91.3% —
# the single strongest predictor in the entire dataset.
# Re-target past subscribers before going after new leads.
```

---

## 8. Plot 5 — Age Distribution by Outcome

```python
fig, ax = plt.subplots(figsize=(9, 6))
ax.hist(df[df['subscribed']==1]['age'], bins=30, alpha=0.7,
        color=ACCENT, label='Subscribed')
ax.hist(df[df['subscribed']==0]['age'], bins=30, alpha=0.7,
        color=WARN, label='Not Subscribed')
ax.set_title("Age Distribution by Outcome", fontsize=14, fontweight='bold', pad=12)
ax.set_xlabel("Age")
ax.set_ylabel("Count")
ax.legend()
plt.tight_layout()
plt.savefig("plots/plot5_age_distribution.png", dpi=150, bbox_inches='tight')
plt.show()

# INSIGHT: Almost no age difference between groups (avg 41.7 vs 40.8 years).
# Age alone is NOT a useful predictor of subscription.
# Job type and account balance are far stronger indicators.
```
