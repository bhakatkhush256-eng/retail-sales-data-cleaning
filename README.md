# Bazaar Basket Retail Sales & Regional Performance Analysis 2025


---

## 1. Project Background

Bazaar Basket is a fictional multi-category retail chain selling in four regions of Pakistan: Punjab, Sindh, KPK and Balochistan. Branches enter orders into a CRM in their own way, so the 2025 sales export is messy.

The Head of Retail Operations has a budget decision to make for next quarter and asked two things: where is the business really performing, and are discounts hurting some regions?

**Who will use this analysis:**
- Retail Operations: regional budget and staffing
- Merchandising: category stock levels
- Marketing and Promotions: discount and campaign decisions

**The business problem:** regional sales feel uneven and discounts keep growing, but nobody can trust the raw export. This project first cleans the data, then answers the questions with the cleaned numbers.

**Scope:** 400 raw order rows, January to December 2025. After cleaning, 349 valid rows are used. All amounts are in PKR.

---

## 2. Executive Summary

In 2025 the valid orders generated **PKR 8.69M in net revenue** from 349 orders (average order value PKR 24,894). **Punjab produced 52.7% of it** and **Electronics produced 50.0%** while making up only 23.5% of orders. Monthly revenue was volatile, peaking at PKR 1.32M in February and falling to PKR 0.31M in May, and the second half of the year was flat against the first (+0.4%).

Discounts cost PKR 0.68M, or 7.3% of gross sales. **Punjab and KPK account for 74.6% of all discount value**, and KPK has the highest discount rate at 8.4%. Returned and cancelled orders hold PKR 0.99M, or 11.4% of net revenue, so the headline figure overstates what the business can rely on.

**Recommended focus for next quarter:** protect Punjab and Electronics, test a lower discount in KPK, and report revenue after returns and cancellations.

---

## 3. Data Structure

One order table of 400 rows and 13 raw fields. Raw_Data is never edited. Clean_Data rebuilds every field with formulas and adds 6 columns. A small City to Region lookup table recovers blank regions.


The same structure in DBML (paste it into [dbdiagram.io](https://dbdiagram.io) to view it): [data_structure.dbml](data_structure.dbml)

**Workbook:** [Retail_Sales_Regional_Performance.xlsx](Retail_Sales_Regional_Performance.xlsx)

| Sheet | What it holds |
|---|---|
| Raw_Data | The untouched messy export |
| Clean_Data | Formula-cleaned data. Click any cell to see the formula used |
| Data_Dictionary | Every column, the problem found, the fix, a cleaning summary and assumptions |
| Analysis | SUMIFS tables behind every number in this README, plus a line chart |

---

## 4. Data Cleaning Summary

Of 400 raw rows, **349 are valid**, 20 are duplicate orders and 31 are invalid because the date, quantity or price could not be read. No row was deleted. Every excluded row carries a reason.

![Before and after](images/05_before_after.png)

| Issue | Scale | Fix |
|---|---|---|
| Dates in four formats | 255 of 400 stored as text, 8 blank | Rebuilt with `DATE`, unreadable dates flagged |
| Prices typed with "Rs.", "PKR" and commas | 198 rows | `SUBSTITUTE` and `VALUE` |
| Misspelled or abbreviated region, category, payment and status | All four fields | `INDEX`/`MATCH` against fixed lists |
| Blank regions | 23 rows | Recovered from City with a lookup table |
| Negative quantities | 6 rows | Treated as typing mistakes with `ABS` |
| Duplicate orders under different ID spellings | 20 rows | `TRIM`, `UPPER`, then `COUNTIF` to flag repeats |

Invalid rows: 15 for quantity, 8 for price, 8 for date. Full detail is in the Data_Dictionary sheet.

---

## 5. Insights Deep Dive

All figures use valid rows only and come from the Analysis sheet.

### 5.1 Punjab drives more than half of revenue


| Region | Orders | Net revenue (PKR) | Share | Avg order value (PKR) |
|---|---|---|---|---|
| Punjab | 196 | 4,574,323 | 52.7% | 23,338 |
| Sindh | 73 | 2,269,375 | 26.1% | 31,087 |
| KPK | 51 | 1,240,288 | 14.3% | 24,319 |
| Balochistan | 29 | 604,128 | 7.0% | 20,832 |

**I found** that Punjab earns 52.7% of net revenue and leads in 9 of 12 months. **This means** the business depends heavily on one region. Sindh is the only region with a clearly higher order value (PKR 31,087 against the PKR 24,894 average), so it earns more per order, not just from volume.

### 5.2 Revenue is volatile, and the year ended flat



**I found** that net revenue jumped 290% from January (PKR 0.34M) to February (PKR 1.32M), then fell 76% by May (PKR 0.31M). December rebounded 61% month over month to PKR 1.01M. February and March together hold 27.6% of annual revenue. First half (PKR 4.34M) and second half (PKR 4.35M) differ by only 0.4%. **This means** there is no growth trend in 2025, only swings, and stock and promotions need to be timed around the peaks and the May low.



### 5.3 Electronics produce half the revenue

| Category | Orders | Net revenue (PKR) | Share | Discount rate |
|---|---|---|---|---|
| Electronics | 82 | 4,340,090 | 50.0% | 7.3% |
| Clothing | 89 | 1,928,245 | 22.2% | 8.5% |
| Home & Kitchen | 53 | 1,374,565 | 15.8% | 6.7% |
| Groceries | 76 | 446,290 | 5.1% | 7.1% |
| Beauty | 43 | 412,523 | 4.7% | 5.4% |
| Uncategorised | 6 | 186,400 | 2.1% | 2.0% |

**I found** that Electronics bring in 50.0% of net revenue from 23.5% of orders, while Groceries and Beauty together bring in 9.9% from 34% of orders. **This means** a stock-out in Electronics costs far more than one in any other category. Clothing has the highest discount rate (8.5%) on the second-largest revenue.

### 5.4 Discounts are concentrated in Punjab and KPK

| Region | Discount (PKR) | Discount rate (% of gross sales) |
|---|---|---|
| KPK | 114,363 | 8.4% |
| Punjab | 394,928 | 7.9% |
| Sindh | 144,625 | 6.0% |
| Balochistan | 28,873 | 4.6% |
| **Total** | **682,788** | **7.3%** |

**I found** that discounts cost PKR 682,788 (7.3% of gross sales) and that Punjab and KPK together hold PKR 509,290 of it (74.6%). KPK discounts the deepest while earning only 14.3% of revenue. **This means** discount spending is not proportional to the revenue each region earns. This data cannot show whether the discounts caused extra sales.

### 5.5 One in nine rupees sits in returned or cancelled orders

| Status | Orders | Net revenue (PKR) | Share |
|---|---|---|---|
| Completed | 290 | 7,407,498 | 85.3% |
| Pending | 17 | 289,843 | 3.3% |
| Returned | 24 | 367,798 | 4.2% |
| Cancelled | 18 | 622,975 | 7.2% |

| Region | Returned or cancelled orders | Rate |
|---|---|---|
| Balochistan | 5 of 29 | 17.2% |
| Sindh | 10 of 73 | 13.7% |
| Punjab | 24 of 196 | 12.2% |
| KPK | 3 of 51 | 5.9% |

**I found** that returned and cancelled orders hold PKR 990,773, which is 11.4% of net revenue, and that Balochistan has the highest rate (17.2%) against 12.0% overall. **This means** headline net revenue overstates the revenue the business keeps, and Balochistan deserves a closer look, but its sample is small (29 orders).

### 5.6 Sales rep contribution

Nimra Aslam leads with PKR 1.69M (19.5% of net revenue) and Waqar Ahmed is lowest among named reps at PKR 1.12M (12.9%). Eleven valid orders (PKR 247,450, 2.8%) have no sales rep recorded.

---

## 6. Recommendations

1. **Protect Punjab and Electronics first.** Punjab earns 52.7% of net revenue and Electronics 50.0%. Secure Electronics stock in Punjab before spending on any other region or category.
2. **Test a lower discount in KPK.** KPK has the highest rate (8.4%) against 6.0% in Sindh. Cap KPK at 6.0% for one month in one category and compare order counts. At the current volume that would save about PKR 33,000, before any change in sales.
3. **Review Punjab discounts next.** Punjab holds PKR 394,928 of the PKR 682,788 discount total (57.8%). Check which products and discount tiers drive it before changing anything.
4. **Plan stock and promotions around the swings.** Build stock ahead of February and December. Use promotions in April and May to lift the low period (May: PKR 0.31M).
5. **Report revenue after returns and cancellations.** PKR 990,773 (11.4%) of net revenue is in returned or cancelled orders. Base next quarter's budget on completed revenue (PKR 7.41M) plus pending orders.
6. **Investigate Balochistan returns before cutting its budget.** Its 17.2% return and cancel rate is the highest, but it is only 7.0% of revenue and 29 orders. Collect the return reasons first.
7. **Fix the data at the source.** Use dropdown lists for region, category, payment and status, one date format, numeric-only prices and required fields. This would remove most of the cleaning work.

---

## 7. Clarifying Questions, Assumptions and Caveats

### Questions I would ask the stakeholder
- Should Net Revenue exclude returned and cancelled orders? Are pending orders counted as revenue?
- Are negative quantities returns or typing mistakes?
- Is discount depth set by region, by product or by the branch?
- Do you have cost or margin data? Revenue alone cannot show profitability.
- Is 2024 data available so growth can be compared year over year?
- Do the four regions match how sales teams are organised?

### Assumptions
- The first appearance of an Order ID is kept and later repeats are marked Duplicate.
- Negative quantities were made positive. A blank discount is treated as 0%.
- Region is recovered from City only when the city is known.
- Rows with an unreadable date, quantity or price are excluded from totals rather than guessed.
- Returned and Cancelled orders stay in net revenue figures and are shown separately so the stakeholder can decide.


---

## 8. Next Steps

1. **Build a Power BI dashboard** with KPI cards (Net Revenue, Orders, Average Order Value), a monthly trend line and region and category slicers.
2. **Automate the cleaning with Power Query** so a new CRM export is cleaned with one refresh.
3. **Repeat the cleaning in Python (pandas)** and confirm the same 349 valid rows.
4. **Load the clean data into SQL** and answer the same questions with `GROUP BY` and window functions, for example ranking regions by net revenue each month.
5. **Add cost data** to measure margin and test whether discounts pay off.

---

**Contact:** [LinkedIn](https://linkedin.com/in/khush-bakhat-97ba07394) | [GitHub](https://github.com/bhakatkhush256-eng) | bhakatkhush256@gmail.com
