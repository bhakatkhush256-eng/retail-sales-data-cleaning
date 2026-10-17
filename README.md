# Retail Sales & Regional Performance: Data Cleaning in Excel


---

## 1. Overview

A retail chain sells across four regions of Pakistan (Punjab, Sindh, KPK and Balochistan). Branches enter orders into a CRM in their own way, and the export is messy. The Head of Retail Operations wants to know where the business is really performing and whether growing discounts are hurting some regions, so she can decide where to put next quarter's inventory and promotion budget.

None of those questions can be answered reliably from the raw export. This project is the first step: turning 400 messy order rows into a trusted, documented dataset that is ready for analysis.

**The decision this supports:** which regions and categories get priority for next quarter's budget, and whether discounting should be reduced in any region.

**Metrics the clean data was built to support:** Net Revenue, Gross Sales, Discount Rate, Order Count, Average Order Value, Return/Cancel Rate, month-over-month growth and revenue share by region.

---

## 2. Data Structure

One table, 400 rows, 13 raw fields. Raw_Data is never edited. Clean_Data rebuilds every field with formulas and adds 6 new columns.

| Field | Clean type | Notes |
|---|---|---|
| Order ID | Text | Key. Repeats mean duplicate orders |
| Order Date | Date | Jan to Dec 2025 |
| Customer Name | Text | |
| Region | Text | Punjab, Sindh, KPK, Balochistan |
| City | Text | 13 cities, each belongs to one region |
| Category | Text | Electronics, Home & Kitchen, Clothing, Groceries, Beauty |
| Product | Text | 19 products |
| Quantity | Number | |
| Unit Price (PKR) | Number | |
| Discount % | Percentage | |
| Sales Rep | Text | 6 reps |
| Payment Method | Text | Credit Card, Debit Card, Cash, PayPal |
| Order Status | Text | Completed, Returned, Cancelled, Pending |

**Columns added during cleaning:** Gross Sales, Discount Amount, Net Revenue, Order Month, Row Status, Issue Note.

---

## 3. Executive Summary

Of 400 raw rows, **349 are valid and usable**, 20 are duplicate orders, and 31 are invalid because the date, quantity or price could not be read. No row was deleted. Every excluded row is flagged with a reason.

| Metric | Value |
|---|---|
| Total raw rows | 400 |
| Valid rows | 349 |
| Duplicate rows flagged | 20 |
| Invalid rows flagged | 31 |
| Blank regions recovered from city | 23 |
| Net revenue, valid rows (PKR) | 8,688,113 |

Every cleaned value is a live Excel formula pointing back to its raw cell, so any result can be traced and checked.

---

## 4. Cleaning Deep Dive

### Issues found and how each was fixed

| Issue | Scale | Fix |
|---|---|---|
| Dates in four formats (real date, 2025-03-14, 14/03/2025, 14-Mar-2025) | 255 of 400 stored as text, 8 blank | Detect the format and rebuild with `DATE`. Unreadable dates stay empty and are flagged |
| Unit prices typed with "Rs.", "PKR" and commas | 198 rows | Strip labels with `SUBSTITUTE`, convert with `VALUE` |
| Regions, categories, payment methods and statuses misspelled or abbreviated (Pnjab, K.P.K, Electronic, CC, canceled) | Across all four fields | Standardise with `INDEX`/`MATCH` against fixed lists |
| Blank regions | 23 rows | Recover from City using a City to Region lookup table |
| Discounts written as 10%, 0.1 and 10 | Mixed across the column | Convert all to a percentage. Blank is treated as 0% |
| Negative quantities | 6 rows | Treated as typing mistakes and corrected with `ABS` |
| Quantity as "N/A", the word "two" or blank | Part of the 15 quantity-related invalid rows | Left empty and flagged Invalid |
| Duplicate orders under slightly different Order ID spellings | 20 rows | Normalise the ID with `TRIM` and `UPPER`, then flag repeats with `COUNTIF` |
| Names, cities, products and reps with wrong case and extra spaces | Many rows | `TRIM` and `PROPER` |

### Why 31 rows are invalid

| Reason | Rows |
|---|---|
| Quantity missing or unreadable | 15 |
| Unit price missing or unreadable | 8 |
| Date missing or unreadable | 8 |

### Row status logic

- **Valid:** first appearance of the Order ID with a readable date, quantity and price
- **Duplicate - Exclude:** the Order ID already appeared in an earlier row
- **Invalid - Exclude:** date, quantity or price is missing or unreadable

### Calculated columns

- Gross Sales = Quantity x Unit Price
- Discount Amount = Gross Sales x Discount %
- Net Revenue = Gross Sales - Discount Amount
- Order Month = first day of the order's month, for monthly trends

---

## 5. Recommendations

These are about how the data is collected, based on the problems found.

1. **Use dropdown lists** for region, category, payment method and order status. This would remove nearly all the spelling and abbreviation problems.
2. **Force one date format** in the CRM entry form. Four formats in one column is the biggest source of cleaning work.
3. **Store price as a number only** and put the currency in the column header, not in the cell.
4. **Make Order ID unique and required**, so repeated orders cannot be entered twice.
5. **Reject zero or negative quantities** at entry, and make date, quantity, price and region required fields.

---


## 7. Files

- `Retail_Sales_Regional_Performance.xlsx`
  - `Raw_Data`: the untouched messy export
  - `Clean_Data`: formula-cleaned data. Click any cell to see the formula used
  - `Data_Dictionary`: every column, the problem found, the fix, a cleaning summary and assumptions
- `2_before_after_mockup.png`: before and after view

## 8. Next Steps

Analyse the clean data with `SUMIFS` and PivotTables (filter Row Status to Valid): net revenue by region, monthly trend, discount impact by region and top sales reps.

---

**Contact:** [LinkedIn](https://linkedin.com/in/khush-bakhat-97ba07394) | [GitHub](https://github.com/bhakatkhush256-eng) | bhakatkhush256@gmail.com
