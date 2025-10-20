# Campaign ROI Prioritization: Detailed Methodology

## Data Preparation Process

### Power Query Steps

**Query 1: Load Campaign Data**
```
Source = Csv.Document(File.Contents("campaign_ranking_by_roi.csv"))
Promoted Headers = Table.PromoteHeaders(Source)
Changed Types = Table.TransformColumnTypes(Promoted Headers, {
    {"Campaign_ID", type text},
    {"Campaign_Name", type text},
    {"Category", type text},
    {"Spend", Currency.Type},
    {"Revenue", Currency.Type},
    {"ROI", type number}
})
Cleaned = Table.SelectRows(Changed Types, each [Spend] > 0)
```

**Query 2: Calculate Metrics**
```
Added ROI Tier = Table.AddColumn(Cleaned, "ROI_Tier", 
    each if [ROI] >= 2.5 then "High"
    else if [ROI] >= 1.5 then "Good"
    else if [ROI] >= 1.0 then "Marginal"
    else "Underperformer")

Added ROI per Euro = Table.AddColumn(Previous Step, "ROI_per_Euro",
    each [Revenue] / [Spend])
```

---

## Excel Formulas Reference

### Key Calculations

**Total ROI:**
```excel
=SUMIF(Campaign_Range, Campaign_ID, Revenue_Range) / 
 SUMIF(Campaign_Range, Campaign_ID, Spend_Range)
```

**ROI Tier Classification:**
```excel
=IF([@ROI]>=2.5,"High",
  IF([@ROI]>=1.5,"Good",
    IF([@ROI]>=1,"Marginal","Cut")))
```

**Rank Campaigns by ROI:**
```excel
=RANK.EQ([@ROI], ROI_Column, 0)
```

**Budget Utilization %:**
```excel
=[@Spend] / $Total_Budget$
```

**Incremental Revenue per Campaign:**
```excel
=[@Revenue] - [@Spend]
```

---

## Solver Configuration

### Optimization Model Setup

**Objective Cell:** `=SUMPRODUCT(Proposed_Spend, Campaign_ROI)`

**Variable Cells:** `Proposed_Spend` range (B2:B51)

**Constraints:**
1. `SUM(Proposed_Spend) = 500000` (Total budget)
2. `Proposed_Spend >= 5000` (Minimum per campaign)
3. `Proposed_Spend <= 100000` (Maximum per campaign)
4. `COUNTIFS(Category, "A", Proposed_Spend, ">0") >= 1` (Category presence)

**Solver Options:**
- Method: GRG Nonlinear
- Max Time: 100 seconds
- Iterations: 1000
- Precision: 0.000001
- Constraint Precision: 0.000001

---

## Dashboard Design

### Key Components

**1. ROI Distribution Chart**
- Type: Column chart
- X-axis: Campaign names (sorted by ROI descending)
- Y-axis: ROI multiple
- Color coding: Conditional formatting by tier

**2. Budget vs ROI Scatter Plot**
- X-axis: Current spend
- Y-axis: ROI
- Bubble size: Revenue
- Quadrant lines at ROI = 1.5 and Spend = €10K

**3. Waterfall Chart**
- Starting point: Current total revenue
- Decrements: Revenue lost from cuts
- Increments: Revenue gained from increases
- Ending point: Optimized total revenue

**4. Category Heatmap**
- Rows: Categories
- Columns: ROI tiers
- Cell values: Count of campaigns
- Conditional formatting: Green (good) to Red (poor)

---

## Validation Checks

### Data Quality Rules

**Rule 1: ROI Consistency**
```excel
=IF(ABS((Revenue/Spend) - ROI) > 0.01, "CHECK", "OK")
```

**Rule 2: Budget Sum**
```excel
=IF(SUM(Proposed_Spend) <> 500000, "ERROR: Budget mismatch", "OK")
```

**Rule 3: Category Presence**
```excel
=IF(COUNTIFS(Category, A2, Proposed_Spend, ">0") = 0, 
    "WARNING: Category excluded", "OK")
```

---

## Scenario Analysis Framework

### Scenario Comparison Table

| Scenario | Reallocation Allowed | Risk Level | Expected ROI | Implementation Time |
|----------|---------------------|------------|--------------|---------------------|
| Conservative | 20% | Low | +12% | 1 month |
| Balanced | 50% | Medium | +15% | 2 months |
| Aggressive | 100% | High | +18% | 3 months |

### Scenario Switch Formula
```excel
=CHOOSE($Scenario_Selection$,
    Current_Spend * 0.8 + Optimal_Spend * 0.2,    // Conservative
    Current_Spend * 0.5 + Optimal_Spend * 0.5,    // Balanced
    Optimal_Spend)                                 // Aggressive
```

---

## Sensitivity Analysis

### Key Variables Tested

1. **Total Budget Changes:** ±10%, ±20%
2. **ROI Assumptions:** ±15% on all campaigns
3. **Minimum Campaign Spend:** €3K, €5K, €10K
4. **Category Constraints:** 1, 2, or 3 campaigns minimum

### Results Table
```
Budget Change | Optimal ROI | Campaigns Funded
-20%          | 2.25x       | 38
-10%          | 2.18x       | 42
 0% (base)    | 2.10x       | 45
+10%          | 2.05x       | 48
+20%          | 1.98x       | 50
```

**Insight:** Returns diminish as budget increases beyond current level

---

## Implementation Notes

### Phased Rollout Approach

**Phase 1 (Month 1):** Quick wins
- Cut 5 worst performers immediately
- Shift €55K to proven winners
- Risk: Low | Expected lift: +8%

**Phase 2 (Months 2-3):** Major reallocation
- Gradually reduce 10 marginal campaigns
- Test increased budgets on high performers
- Risk: Medium | Expected lift: +7% additional

**Phase 3 (Months 4-6):** Full optimization
- Complete transition to optimized allocation
- Launch new campaigns in high-ROI categories
- Risk: Medium-High | Expected lift: +3% additional

---

## Tools & Techniques Summary

**Excel Functions Used:**
- SUMIFS, COUNTIFS, AVERAGEIFS
- INDEX, MATCH, XLOOKUP
- IF, AND, OR nested logic
- RANK, LARGE, SMALL
- SUMPRODUCT, MMULT
- Named ranges and tables

**Power Query Transformations:**
- Merge queries
- Append queries
- Pivot/unpivot columns
- Group by aggregations
- Custom columns with M code

**Advanced Excel Features:**
- Solver add-in
- Scenario Manager
- Data Tables (What-If)
- Conditional formatting with formulas
- Dynamic named ranges
- Pivot Tables with calculated fields
- Slicers for interactivity

---

**Document Version:** 1.0  
**Last Updated:** January 2025  
**Author:** Arkaprabha Ray
