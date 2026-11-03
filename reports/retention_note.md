# Note to the Head of Customer Retention

**From:** Data Science, Customer Retention team · **Subject:** Where to spend the retention budget

## The short version

Stop the blanket discount. Last month **26.5%** of customers left (1,869 of 7,043), but churn is heavily concentrated. A model that ranks customers by risk lets us aim the budget at the **15% of customers who account for 40% of leavers**, and largely ignore the 60% who almost all stay.

## Who leaves

| Group | Churn rate |
|---|---|
| **Month-to-month + fibre-optic internet** | **54.6%** (2,128 customers) |
| Month-to-month contract (all) | 42.7% — vs 11.3% one-year and 2.8% two-year |
| First year with us (0–12 months) | 47.4% — vs 9.5% after four years |
| Fibre-optic internet | 41.9% — vs 19.0% DSL |
| Pays by electronic check | 45.3% — vs 15–19% for other methods |
| No online security / no tech support | ~42% — vs ~15% with them |

Churners also pay about **$13 more per month** on average ($74 vs $61). That gap is statistically real, but it largely reflects that churners are on fibre plans; it does not prove price itself is the reason they leave.

## Who to target first

| Risk band | Share of customers | Actually left | Share of all leavers |
|---|---|---|---|
| **High** (score ≥ 60%) | 15% | **72%** | 40% |
| **Medium** (30–60%) | 24% | 40% | 35% |
| **Low** (< 30%) | 61% | 11% | 25% |

*Measured on 1,409 customers the model had never seen.*

The **High** band is a clear profile: every one is month-to-month, 95% are on fibre, median tenure is 4 months, ~4 in 5 pay by electronic check, and almost none have online security or tech support.

**Recommendation:** spend first on High, then Medium. Remove the blanket discount for Low.

**Offer to test:** for High-risk customers, a discount or a free security/tech-support add-on **in exchange for moving to a one-year contract**. Contract length is the strongest lever in the data, and missing support services is common in the risky group. Also nudge electronic-check payers towards automatic payment.

**Run it as an experiment:** randomly hold back about 20% of High-risk customers as a control group. The difference in churn between the two groups is the number of customers the offer actually saved, which is what tells us whether it pays for itself.

## How the model would be used

1. Each month, score every current customer with the model (a churn probability between 0 and 1).
2. Put each customer into Low / Medium / High.
3. Send the High list (sorted by score) to the retention team; work down the list until the budget runs out.
4. Track who accepts the offer and who leaves, and retrain the model every quarter.

The model ranks customers well (ROC-AUC 0.84, where 0.5 is guessing). Note that simply predicting "nobody leaves" is already 73.5% accurate, so accuracy alone is a misleading measure here.

## Limits

- **It predicts who will leave, not who can be saved.** Some High-risk customers will leave whatever we offer. Only the control-group test shows the offer's real effect.
- **It doesn't explain why.** We have no complaints, network quality, usage or competitor-price data. Patterns such as "fibre customers leave more" need investigating (price? reliability? competition?).
- **One month of data.** No seasonality and no trend, and nothing to confirm the model will hold next quarter. It must be monitored and retrained.
- **No customer value.** We can't yet tell whether a saved customer is worth more than the offer costs. Margin or lifetime value should be added before scaling up.
- **Sample data.** This analysis uses IBM's fictional sample in US dollars; results must be re-checked on Simu Connect's own customers before rollout.

*Full analysis: `notebooks/churn_analysis.ipynb`.*
