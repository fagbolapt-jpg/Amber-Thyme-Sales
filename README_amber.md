# Amber & Thyme: Restaurant Sales Analysis (Excel)

A descriptive analysis of three months of order data from a fictional restaurant in Lagos, built end to end in Excel: cleaning in Power Query, analysis in PivotTables, and a dashboard.

> **Note:** Amber & Thyme is not a real restaurant. The dataset is synthetic. This project shows how I would approach a business problem like this as a data analyst, not discoveries about a real business.

---

## Project summary

The owner of Amber & Thyme feels that sales are inconsistent but has never had the numbers analysed. I cleaned a raw point-of-sale export of 3,970 rows (January to March 2026) down to 3,754 valid orders and looked at where and when sales are weak.

**Headline finding:** the afternoon (2pm to 5pm) is the weakest window on every day of the week, and it is deepest from Monday to Wednesday. Those afternoons have both fewer orders and smaller orders. Lunch and dinner hold up well on every day. Order type and party size do not explain the dip.

The data can show where and when sales dip. It cannot show why, so I recommend a controlled test rather than claiming a cause.

---

## Introduction

Amber & Thyme is a fictional mid-range restaurant in Lekki, Lagos, open seven days a week with dine-in, takeout and delivery. I chose a synthetic dataset on purpose, so I could work through a realistic, messy first-time dataset, the kind a business hands over when it has never worked with an analyst before.

---

## Problem statement

Over the past few months, the owner has noticed that daily revenue feels inconsistent but has no clear sense of why. Some days feel unusually quiet, and it is unclear whether that is tied to specific days of the week, times of day, or something else. The restaurant's only record of transactions is a raw export from its point-of-sale (POS) system for January to March 2026, with no prior cleaning or analysis.

**What the owner wants:** to know which days and time periods are underperforming, and to get a clear, practical recommendation.

---

## Data overview

**Source:** synthetic raw POS export, generated with AI assistance to mimic the quality problems of a real first-time dataset. 3,970 rows, 10 columns, covering 1 January to 31 March 2026.

| Column | What it holds | Type in the cleaned data |
|---|---|---|
| Order_ID | Unique order number | Whole number |
| Date | Order date (arrived in five different text formats) | Date |
| Time | Order time (mixed 12-hour and 24-hour) | Time |
| Day | Weekday as recorded (inconsistent, for example "Fri" and "FRIDAY") | Replaced by a column built from the cleaned date |
| Order_Type | Dine in, takeout or delivery (spelled several ways) | Text |
| Party_Size | Number of guests | Text, so "Unknown" can be kept |
| Items_Ordered | Items in the order | Text |
| Revenue_NGN | Order value in naira | Decimal |
| Payment_Method | Cash, card, transfer or POS terminal | Text |
| Server | Staff member who took the order | Text |

**Columns I added:** Month, Day (rebuilt from the cleaned date) and Party_Size_Numeric (a numeric version of party size so averages work).

---

## Data cleaning

I cleaned the data in Power Query so every step is recorded and repeatable.

| Issue | What I found | What I did | Why |
|---|---|---|---|
| Exact duplicates | 58 rows | Removed | Double entries |
| Missing revenue | 158 rows, including 6 garbled "SYSTEM ERROR" rows with a 0000-00-00 date | Removed | Revenue is the number I sum and average, and guessing it would distort the results |
| Negative revenue | 62 rows in the raw file | Checked them first: items, payment and everything else were present, so they were real orders with a sign error. I converted them to positive values | Dropping real orders would lose sales |
| Mixed date formats | Five formats, plus slash dates that could be read day-first or month-first | Tried both readings and kept the one that matched the recorded weekday and fell inside January to March 2026 | Avoids silently swapping days and months |
| Day column | Inconsistent spellings and abbreviations | Rebuilt from the cleaned date | A calculated value cannot have typos |
| Time column | Mixed 12-hour and 24-hour | Converted to one time type | Needed to group orders by hour |
| Missing party size | 197 rows in the cleaned data | Set to "Unknown" | Keeps the order's revenue in the analysis |
| Missing payment method and server | Blanks | Set to "N/A" | Same reason |
| Text inconsistencies | Spacing, capitalisation and spelling variants in Order_Type and Payment_Method | Trimmed, standardised capitalisation, merged variants (for example "Take out" to "Takeout", "Dine-in" to "Dine In") | One label per category |

**Row counts:** 3,970 raw, 3,912 after removing duplicates, 3,754 after removing missing revenue.

---

## Analysis

I worked in PivotTables, using heatmap-style conditional formatting to spot patterns by hour. The questions I asked:

1. Which days bring in the most and least revenue and orders?
2. Where inside each day is the weak point? (day by hour)
3. Is the weak window specific to one order type?
4. Are the weak hours weak because of fewer orders, smaller orders, or both? (average order value by day and hour)
5. Does party size explain it?

---

## Key insights

1. **The afternoon is the weak window every day.** Only 14% of orders (527 of 3,754) fall between 2pm and 5pm.
2. **It is deepest early in the week.** Monday to Wednesday afternoons averaged about 4 orders a day, against about 8 a day from Thursday to Saturday. Monday at 3pm is the quietest hour in the dataset, with 9 orders across the whole quarter.
3. **Afternoon orders are also smaller.** The average order outside the afternoon is about ₦13,150 to ₦13,860. In the afternoon it falls to between ₦9,080 (Wednesday, the lowest) and ₦11,880. So the dip is fewer orders and smaller orders.
4. **Lunch and dinner hold up on every day.** Peak trading is Saturday at 12pm (147 orders). Revenue is highest on Saturday (₦10.15m) and lowest on Sunday (₦3.84m). Sunday closes at 8pm, so part of that gap is fewer trading hours, not weak demand.
5. **Order type and party size do not explain the dip.** The mix of delivery, dine-in and takeout looks similar across days, and party size showed no clear link.

**Totals for the quarter:** ₦49.73m revenue, 3,754 orders, ₦13,247.83 average order value.

---

## Dashboard

![Amber & Thyme restaurant performance dashboard](images/dashboard.png)

**How to read it**

- **Top cards:** total revenue, total orders, average order value and the peak period.
- **Orders by day and time (heatmap):** green means busy, yellow means quiet. The afternoon columns (2pm to 4pm) are the pale band across every row, and Monday to Wednesday are the palest.
- **Revenue by day:** shows how much of the weekly revenue each day brings in.
- **Revenue by day and time:** the same heatmap in naira.
- **Revenue by day and order type, and order type distribution:** show that the order type mix does not change much from day to day.
- **Orders by party size:** most orders are parties of two to four.

Sunday has no cells after 8pm because the restaurant closes then.

---

## Recommendations

1. **Test an afternoon offer, Monday to Wednesday, 2pm to 5pm,** for four to six weeks and compare against the baseline of about 4 orders a day and an average order of about ₦10,500. If those afternoons reached Thursday to Saturday's rate, that would mean roughly 150 extra orders and about ₦1.6m a quarter, around 3% of revenue. This is an estimate built on assumptions, not a forecast.
2. **Design the offer around spend per order, not deep discounts.** Afternoon orders are already smaller, so a main-plus-drink bundle makes more sense than cutting prices.
3. **Check staffing and prep for Monday to Wednesday afternoons against this pattern.** At about 4 orders a day the restaurant may be over-covered. I have no rota data, so this is something to check, not a finding.
4. **Start tracking staffing, promotions and customer IDs.** That would let a future analysis look for causes.

---

## Conclusion

Amber & Thyme's sales gap is a weekday afternoon lull that is deepest from Monday to Wednesday, driven by fewer and smaller orders. The data shows where and when the problem happens but not why, so the best next step is a controlled test of an afternoon offer, measured against the baseline in this analysis.

---

## Limitations and assumptions

- **Synthetic data.** The patterns in it were designed in, so this project demonstrates method, not real-world discovery.
- **No information on causes.** The export has no staffing, promotions, foot traffic or competitor data, so I could not test reasons for the dip.
- **Ambiguous dates.** I resolved day/month ambiguity using the recorded weekday and the January to March range. This works because the export covers one known quarter.
- **Quirks of the generated data.** There are no orders between 5pm and 6pm, Sunday's 7pm count (108) is inflated because orders after the 8pm close were logged in the 7pm hour, and the dine-in share (about 43%) reflects how the data was generated. I did not draw conclusions from these.
- **Missing values.** Rows with unknown party size, payment method or server stay in the data, so any analysis of those fields excludes them.

---

## Validation and lessons

Two problems showed up during the project, and checking my own work caught both:

- **Misread dates.** My first date step read some day/month dates the wrong way round, which pushed about 5% of orders into April to December. I noticed because a three-month export should not contain those months. I traced it and fixed it with the weekday check described above.
- **An accidental filter.** A stray filter on party size removed 710 rows. My cleaned table had 3,044 rows when the cleaning steps should have left 3,754. Comparing against the expected count caught it.

**Lesson:** reconcile row counts after every cleaning step, and sanity-check dates against what you know about the data.

---

## Tools

Excel (Power Query, PivotTables, conditional formatting, dashboard).

---

## Repository contents

Adjust these to match your folders.

```
amber-and-thyme-sales-analysis/
├── README.md
├── images/
│   └── dashboard.png
├── data/
│   └── amber_and_thyme_raw_original.csv
└── excel/
    └── (your Excel workbook)
```

---

## About

Built by Patricia, a data analyst based in Lagos, Nigeria.

- LinkedIn: https://www.linkedin.com/in/patricia-fagbola-656566387/
- GitHub: https://github.com/fagbolapt-jpg
