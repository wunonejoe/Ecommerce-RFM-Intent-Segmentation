# RFM-I: Intent-Aware Customer Segmentation for Coupon Targeting

*E-Commerce Customer Segmentation Using a Modified RFM Model*

📊 **[See the results →](RESULTS.md)** — headline numbers and key charts, no code required.

## Problem

Coupon budgets are limited, so targeting has to answer one question well: which
customers will a coupon actually move? Traditional RFM (Recency, Frequency,
Monetary) segmentation answers a different question — who has historically
spent the most — and that gap creates two costly errors:

- **Intent blindness.** A customer who browses heavily and shows real purchase
  intent but hasn't converted yet looks identical to a disengaged customer in
  RFM. Both score low, so both get deprioritized.
- **Income blindness.** RFM treats a dollar as a dollar. A $1,000 order means
  something very different to a high-income shopper than to a low-income one,
  so RFM systematically under-ranks high-income customers relative to their
  real commercial potential.

The result: high-potential customers get miscategorized as low-priority, and
coupon spend either gets spread too thin across everyone or over-concentrated
on already-loyal customers who would have bought anyway.

## Approach: RFM-I

RFM-I extends the classic model with engineered features that capture intent,
friction, activation, and income, combined into a single composite score:

| Signal | What it captures | Engineered from |
|---|---|---|
| **R, F, M** | Standard recency, frequency, monetary scores | Last login, purchase frequency, total spend |
| **I (Intent)** | Depth of purchase intent | Time on site, pages viewed |
| **Friction** | Browsing effort relative to conversion | Pages viewed / purchase frequency |
| **L (Loyalty)** | Active connection to the platform | Newsletter subscription, recent login |
| **Income Level** | Purchasing power context | Income, adjusted against spend |

Guided by correlation and distribution analysis (see the EDA section of the
notebook), these signals are normalized and combined into a `Final_Score` that
re-weights the base RFM score by intent, so high-intent customers are no
longer invisible to the model.

## What it found

Segmenting on RFM-I surfaced 6 customer segments that standard RFM misses
entirely, including:

- **Hesitant Big Spenders** — high income, high intent, low purchase frequency
- **High-Potential Churn Customers** — high income, high historical spend, recently lapsed

These are customers RFM would have written off as low-value or low-priority,
even though they carry real, identifiable upside.

## Validation

The model was validated with a simulated A/B test comparing coupon allocation
under traditional RFM vs. RFM-I targeting, at equal or lower spend:

- **Marginal coupon ROI: 4.0% → 33.5%** (≈8x improvement)
- **20% lower spend** than the traditional targeting strategy

## Repo structure

```
.
├── RFM_Customer_Segmentation.ipynb   # Full analysis: EDA → feature engineering → model → segmentation → A/B test
├── RESULTS.md                        # Headline results and key charts, no code required
├── data/data.csv                     # Customer dataset used in the analysis
├── output/                           # Generated charts land here when the notebook runs
├── assets/                           # Chart images referenced by RESULTS.md
├── requirements.txt
└── README.md
```

## Notebook walkthrough

1. **Setup** — imports and path configuration
2. **Data** — load and quality-check the raw customer dataset
3. **Feature engineering** — build Intent, Friction, Loyalty, and Income-adjusted features
4. **EDA** — distribution and correlation analysis that motivates the model's weights
5. **Model** — construct the RFM-I composite score
6. **Segmentation** — classify users into fine-grained segments, with radar-chart profiles
7. **A/B testing** — simulate and compare coupon ROI, traditional RFM vs. RFM-I

## Running it locally

```bash
git clone <your-repo-url>
cd <your-repo-name>
pip install -r requirements.txt
jupyter notebook RFM_Customer_Segmentation.ipynb
```

> **Note on data:** `data/data.csv` is included in this repo, with the columns
> the notebook expects: `Age`, `Income`, `Income_Level`, `Total_Spending`,
> `Purchase_Frequency`, `Last_Login_Days_Ago`, `Time_Spent_on_Site_Minutes`,
> `Pages_Viewed`, `Newsletter_Subscription`, `Average_Order_Value`.

## Stack

Python, pandas, NumPy, Matplotlib
