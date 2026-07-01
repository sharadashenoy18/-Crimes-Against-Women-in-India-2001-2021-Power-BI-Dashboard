# Crimes Against Women in India — Power BI Dashboard (2001–2021)

A Power BI dashboard analyzing 20 years of NCRB data on crimes against women across India. The goal was to go beyond national totals and actually understand the patterns — which states consistently dominate the numbers, which crime types are growing, and what the trend looks like year on year.

---

## What's in the dashboard

**Page 1 — National Overview**
Top-line KPIs: total reported cases, year-on-year change %, and a Crime Growth Index that benchmarks each year against 2001. Reported cases increased significantly over the two decades, though part of that reflects better reporting infrastructure rather than purely more incidents.

**Page 2 — State-Level Analysis**
Ranked view of states by total cases and State Crime Share % — which states account for a disproportionate share of national numbers. Drillable by crime type so you can see if a state's ranking changes depending on the offence.

**Page 3 — Crime Type Trends**
How individual crime categories have moved over 20 years. Domestic violence and kidnapping/abduction show the steepest rise. A few categories show declining trends in specific states, which points to regional policy differences worth looking into.

---

## DAX measures

| Measure | What it does |
|---|---|
| `YoY Change %` | Year-on-year % change using VAR to compare current vs previous year in context |
| `State Crime Share %` | Each state's share of national total — uses `ALL()` to remove state filter context |
| `Crime Growth Index` | Indexes each year's count against 2001 as the base year |

Full documentation in `DAX_measure.md`.

---

## Tools used

- Power BI Desktop — data modelling, DAX, dashboard
- Microsoft Excel — initial cleaning and formatting

---

## Data source

National Crime Records Bureau (NCRB), Government of India — [data.gov.in](https://data.gov.in)

---

## Author

Sharada Shenoy — MSc Computer Science, Somaiya University  
[LinkedIn](https://www.linkedin.com/in/sharada-shenoy-665a79275) · [GitHub](https://github.com/sharadashenoy18)
