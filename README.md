# Human Resources Attrition Diagnostic Analytics Dashboard

![Power BI](https://img.shields.io/badge/Power%20BI-Dashboard-F2C811?logo=powerbi&logoColor=black)
![DAX](https://img.shields.io/badge/DAX-Measures-0078D4)
![Power Query](https://img.shields.io/badge/Power%20Query-ETL-217346)

## Table of Contents

- [Executive Overview](#executive-overview)
- [Analytical Objectives](#analytical-objectives)
- [Data Architecture](#data-architecture)
- [Data Processing](#data-processing)
- [DAX Measures](#dax-measures)
- [Dashboard Layout](#dashboard-layout)
- [Key Business Inferences](#key-business-inferences)

---

## Executive Overview

A Human Resources analytics dashboard built in **Power BI Desktop** to analyse workforce attrition. Using descriptive and diagnostic analysis, it tracks historical attrition and isolates the main factors behind turnover across departments, salary tiers, and job satisfaction ratings.

## Analytical Objectives

1. Quantify baseline workforce metrics: headcount, total separations, and overall attrition rate.
2. Diagnose how attrition relates to job satisfaction, compensation level, and department.
3. Identify the main recorded reasons for voluntary separation to support retention planning.

---

## Data Architecture

The model uses a **Star Schema** with one fact table and three dimension tables, connected through one-to-many relationships.

| Table | Type | Description |
|-------|------|-------------|
| `Fact_Attrition` | Fact | Employee exit events, compensation, satisfaction ratings, and keys (`EmployeeID`, `DepartmentID`, `DateKey`) |
| `Dim_Employee` | Dimension | Employee demographic profiles and master records |
| `Dim_Department` | Dimension | Departmental hierarchy and unit classification |
| `Dim_Date` | Dimension | Calendar table for time-intelligence analysis |

---

## Data Processing

- **Query optimisation (Power Query):** Removed non-essential binary sources, promoted headers, and standardised regional locale formats.
- **Locale standardisation:** Applied the English (India) locale so joining and separation dates parse consistently.
- **Calculated attribute, Salary Band:** Grouped monthly earnings into five tiers to reduce data dispersion.

| Salary Band |
|-------------|
| Under ₹50k |
| ₹50k – ₹1L |
| ₹1L – ₹1.5L |
| ₹1.5L – ₹2L |
| Above ₹2L |

---

## DAX Measures

All measures are stored in the `Fact_Attrition` table so they evaluate correctly across related dimensions.

```dax
Total Employees = DISTINCTCOUNT(Fact_Attrition[EmployeeID])
```

```dax
Attrition Count =
CALCULATE(
    COUNTROWS(Fact_Attrition),
    Fact_Attrition[AttritionFlag] = 1
)
```

```dax
Attrition Rate = DIVIDE([Attrition Count], [Total Employees], 0)
```

---

## Dashboard Layout

| # | Component | Description |
|---|-----------|-------------|
| 1 | **Executive KPI Header** | Total headcount, attrition count, and attrition rate |
| 2 | **Satisfaction Diagnostic** | Clustered bar of attrition by job satisfaction tier (scale 1–5) |
| 3 | **Departmental Breakdown** | Horizontal bar of turnover by department |
| 4 | **Compensation Tier Distribution** | Column chart of exits by salary band |
| 5 | **Top Separation Drivers** | Top 5 recorded reasons for departure |

---

## Key Business Inferences

- **Departmental concentration:** The Sales department has the highest volume of voluntary separations.
- **Compensation sensitivity:** Exits cluster in entry-to-mid salary brackets, pointing to possible compensation compression against market benchmarks.
- **Satisfaction correlation:** Low satisfaction tiers show high exit frequency, but mid-tier satisfaction (rating 3) also shows notable turnover, so retention risk extends beyond actively disengaged employees.

---

## Tech Stack

- Microsoft Power BI Desktop
- Power Query (M)
- DAX

## Author

**Sarvesh** — PGDM, Jaipuria Institute of Management, Indore
