# DAX Measures – Crimes Against Women (2001–2021)

This file explains the DAX measures used in the Power BI dashboard.

---

## 1. `Total_Cases`

### DAX

```DAX
Total_Cases = SUM(CrimesOnWomenData[Cases])
```

### Explanation

This measure returns the total number of crime cases against women by summing the `Cases` column.
It is the main measure used across the dashboard and changes dynamically based on filters such as year, state, or crime type.

---

## 2. `YoY Change %`

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

### Explanation

This measure calculates the year-on-year percentage change in total crime cases.
It compares the current year’s total with the previous year to show whether crimes have increased or decreased.

`DIVIDE()` is used to avoid errors when the previous year value is zero.

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

### Explanation

This measure shows how much each state contributes to the total number of crimes at the national level.
It removes the state filter in the denominator so that each state’s share can be compared against the overall total.

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

### Explanation

This measure compares crime cases in a selected year with the base year (earliest year in the selection).
A value greater than 1 indicates an increase compared to the base year, while a value below 1 indicates a decrease.

---

