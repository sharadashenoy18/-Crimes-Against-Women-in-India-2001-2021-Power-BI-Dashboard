# Detailed Explanation of DAX Measures

**Crimes Against Women in India (2001–2021)**

---

## 1. `Total_Cases`

### DAX

```DAX
Total_Cases = SUM(CrimesOnWomenData[Cases])
```

### Detailed Explanation

This measure calculates the **aggregate number of crime cases against women** by summing the `Cases` column from the dataset. It serves as the **base metric** for the entire dashboard and is reused by other derived measures such as growth rates, shares, and indices.

The result dynamically changes based on applied filters like **Year, State, or Crime Type**, making it suitable for use in KPI cards, trend charts, and comparative visuals.

---

## 2. `YoY Change %` (Year-on-Year Percentage Change)

### DAX

```DAX
YoY Change % = 
VAR CurrentYear = MAX(CrimesOnWomenData[Year])

VAR CurrentYearCases =
    CALCULATE(
        SUM(CrimesOnWomenData[Cases]),
        CrimesOnWomenData[Year] = CurrentYear
    )

VAR PreviousYearCases =
    CALCULATE(
        SUM(CrimesOnWomenData[Cases]),
        CrimesOnWomenData[Year] = CurrentYear - 1
    )

RETURN
DIVIDE(
    CurrentYearCases - PreviousYearCases,
    PreviousYearCases,
    0
) * 100
```

### Detailed Explanation

This measure computes the **percentage change in total crime cases compared to the previous year**, enabling temporal trend analysis.

* `CurrentYear` identifies the year currently in context.
* `CurrentYearCases` calculates the total number of cases for that year.
* `PreviousYearCases` retrieves the total cases from the immediately preceding year.
* The final calculation measures the relative increase or decrease using a percentage formula.

The use of `DIVIDE()` ensures **safe handling of zero or missing values**, preventing calculation errors. This measure is particularly useful for identifying **spikes, drops, or stable periods** in crime reporting over time.

---

## 3. `State Crime Share %`

### DAX

```DAX
State Crime Share % = 
DIVIDE(
    SUM(CrimesOnWomenData[Cases]),
    CALCULATE(
        SUM(CrimesOnWomenData[Cases]),
        ALL(CrimesOnWomenData[State])
    )
) * 100
```

### Detailed Explanation

This measure calculates the **proportional contribution of each state** to the total crimes reported at the national level.

* The numerator represents the total cases for the selected state.
* The denominator removes the state-level filter using `ALL()`, computing total cases across **all states**.
* The result is expressed as a percentage for easy comparison.

This measure helps identify **high-contributing states** and is especially effective in **bar charts, maps, and ranking visuals**, where relative contribution is more meaningful than absolute values.

---

## 4. `Crime Growth Index`

### DAX

```DAX
Crime Growth Index = 
VAR BaseYear =
    CALCULATE(
        MIN(CrimesOnWomenData[Year]),
        ALLSELECTED(CrimesOnWomenData)
    )

VAR BaseYearCases =
    CALCULATE(
        SUM(CrimesOnWomenData[Cases]),
        CrimesOnWomenData[Year] = BaseYear
    )

RETURN
DIVIDE(
    SUM(CrimesOnWomenData[Cases]),
    BaseYearCases,
    0
)
```

### Detailed Explanation

The Crime Growth Index measures how crime levels have evolved **relative to a base year** within the selected context.

* `BaseYear` dynamically determines the earliest year in the current selection.
* `BaseYearCases` calculates the total cases reported during that base year.
* The current total cases are divided by the base year total to form an index.

An index value:

* **Equal to 1** → No change from the base year
* **Greater than 1** → Increase in crimes
* **Less than 1** → Decrease in crimes

