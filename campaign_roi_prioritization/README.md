# 📈 Campaign ROI Prioritization

## 🎯 Business Problem

**Context:** Multi-category retail brand running 50+ marketing campaigns across product categories with limited budget

**Challenge:**
- Marketing budget: €500,000 fixed allocation
- Campaigns showing ROI range: 0.8x to 3.5x
- Need data-driven framework to prioritize spend
- Balance ROI optimization with strategic category presence
- Existing allocation not aligned with performance

**Stakeholders:**
- Marketing Director (budget owner)
- Category Managers (campaign owners)
- Finance Team (ROI tracking)
- C-Suite (strategic alignment)

**Goal:** Maximize total marketing ROI while maintaining strategic category representation

---

## 📊 Data Sources

| File | Description | Records | Key Fields |
|------|-------------|---------|------------|
| `campaign_ranking_by_roi.csv` | Campaign performance metrics | 50+ campaigns | Campaign ID, Category, Spend, Revenue, ROI |
| `category_performance.csv` | Category-level summaries | 10 categories | Category, Total Spend, Avg ROI, Campaign Count |
| `promotions_casestudy_dataset.csv` | Detailed promotion data | 500+ SKU-promo combinations | SKU, Promo Type, Lift %, Incremental Revenue |
| `budget_reallocation_proposal.csv` | Optimized allocation output | Final recommendations | Campaign, Current Budget, Proposed Budget, Expected ROI |

---

## 🔧 Analytical Approach

### Phase 1: Data Integration & Cleaning (Excel + Power Query)

**Tools Used:** Power Query, Excel Power Pivot

**Steps:**
1. **Import & Consolidate:**
   - Loaded 4 CSV sources into Power Query
   - Cleaned column headers and data types
   - Removed duplicates and handled nulls

2. **Data Model:**
   - Created relationships between campaigns, categories, and promotions
   - Built calculated columns for incremental metrics
   - Established data quality rules

3. **Key Calculations:**
```
   ROI = (Revenue - Spend) / Spend
   Incremental ROI = (Incremental Revenue - Spend) / Spend
   ROI per € = Revenue / Spend
   Marginal ROI = Change in Revenue / Change in Spend
```

---

### Phase 2: ROI Analysis & Segmentation (Excel Modeling)

**Tools Used:** Excel Pivot Tables, Advanced Formulas, Conditional Formatting

**Analysis:**

1. **Campaign Performance Tiers:**
   - High Performers: ROI > 2.5x (12 campaigns)
   - Good Performers: ROI 1.5-2.5x (18 campaigns)
   - Marginal: ROI 1.0-1.5x (13 campaigns)
   - Underperformers: ROI < 1.0x (7 campaigns)

2. **Category Breakdown:**
   - Pivot analysis of ROI by category
   - Identified top and bottom performing segments
   - Cross-tabulation of spend vs. return

3. **Trend Analysis:**
   - Campaign performance over time
   - Seasonal patterns
   - Declining ROI identification

**Key Formulas Used:**
```excel
=SUMIFS(Revenue, Campaign, A2) / SUMIFS(Spend, Campaign, A2)
=IF(ROI>2.5, "High", IF(ROI>1.5, "Good", IF(ROI>1, "Marginal", "Cut")))
=INDEX(Campaign_Range, MATCH(LARGE(ROI_Range, 1), ROI_Range, 0))
```

---

### Phase 3: Optimization Model (Excel Solver)

**Tools Used:** Excel Solver, Scenario Manager

**Optimization Setup:**

**Objective Function:**
```
Maximize: Total ROI = SUM(Campaign_Spend × Campaign_ROI)
```

**Constraints:**
1. Total budget = €500,000 (hard constraint)
2. Each category must have ≥ 1 campaign (strategic constraint)
3. Minimum spend per campaign = €5,000 (operational constraint)
4. Maximum spend per campaign = €100,000 (risk constraint)

**Solver Parameters:**
- Method: GRG Nonlinear
- Max iterations: 1000
- Precision: 0.0001

**Scenarios Tested:**
1. **Conservative:** Maintain 80% of current allocation
2. **Balanced:** 50% reallocation allowed
3. **Aggressive:** Full optimization (current model)

---

### Phase 4: Visualization & Recommendations (Excel Dashboard)

**Dashboard Components:**

1. **ROI Waterfall Chart:** Shows budget reallocation impact
2. **Category Heatmap:** Performance by category and campaign type
3. **Before/After Comparison:** Current vs. optimized allocation
4. **Cut List:** Campaigns recommended for elimination
5. **Double-Down List:** High-ROI campaigns for increased spend

**Interactivity:**
- Slicers for category filtering
- Scenario toggle (Conservative/Balanced/Aggressive)
- What-if analysis with spin buttons

---

## 💡 Key Insights

### Current State Analysis

**Budget Misalignment:**
- 30% of budget allocated to campaigns with ROI < 1.2x
- Top 20% performers only receiving 25% of budget
- €150K deployed inefficiently

**Category Imbalances:**
- Category A: 8 campaigns, avg ROI 1.2x (overfunded)
- Category B: 3 campaigns, avg ROI 3.1x (underfunded)

**Specific Problem Campaigns:**
- 7 campaigns with negative or near-zero ROI
- Combined spend: €87K with €72K in returns (0.83x ROI)

---

### Optimized Allocation Recommendations

**Reallocation Strategy:**
1. **Cut completely:** 5 campaigns (ROI < 0.9x) - Save €55K
2. **Reduce significantly:** 10 campaigns (ROI 1.0-1.3x) - Save €95K
3. **Maintain:** 23 campaigns (ROI 1.3-2.0x) - Keep current
4. **Increase:** 12 campaigns (ROI > 2.5x) - Add €150K

**Specific Moves:**
- Shift €75K from Brand Awareness to Performance Marketing
- Reduce traditional media spend by 40%
- Double digital campaign budgets for top performers

---

## 📈 Business Impact

### Projected Outcomes (Aggressive Scenario)

| Metric | Current | Optimized | Change |
|--------|---------|-----------|--------|
| **Total Budget** | €500,000 | €500,000 | 0% |
| **Total Revenue** | €890,000 | €1,050,000 | **+€160K (+18%)** |
| **Blended ROI** | 1.78x | 2.10x | **+0.32x (+18%)** |
| **Negative ROI Campaigns** | 7 | 2 | **-71%** |
| **Campaigns Funded** | 50 | 45 | -10% |
| **Avg ROI per Campaign** | 1.78x | 2.33x | **+31%** |

### Risk-Adjusted Returns
- Conservative scenario: +12% ROI improvement
- Balanced scenario: +15% ROI improvement  
- Aggressive scenario: +18% ROI improvement (selected)

---

## 🎯 Implementation Roadmap

### Immediate Actions (Month 1)
- [ ] Cut 5 underperforming campaigns
- [ ] Reallocate €55K to top 5 performers
- [ ] Implement weekly ROI tracking dashboard

### Short-term (Months 2-3)
- [ ] Phase out 10 marginal campaigns gradually
- [ ] A/B test increased budgets on high performers
- [ ] Develop category-specific ROI targets

### Long-term (Months 4-6)
- [ ] Build predictive ROI model
- [ ] Automate budget reallocation triggers
- [ ] Integrate with marketing automation platform

---

## 📁 Files in This Case
```
campaign_roi_prioritization/
├── README.md (you are here)
├── data/
│   ├── campaign_ranking_by_roi.csv
│   ├── category_performance.csv
│   ├── promotions_casestudy_dataset.csv
│   └── budget_reallocation_proposal.csv
├── reports/
│   ├── executive_summary.pdf
│   ├── full_presentation.pdf
│   └── dashboard_screenshots.pdf
└── documentation/
    └── methodology_details.md
```

---

## 🔧 Excel Techniques Demonstrated

**Power Query:**
- Multi-source data consolidation
- Custom column creation
- Data type transformation
- Duplicate removal logic

**Formulas & Functions:**
- SUMIFS, INDEX-MATCH combinations
- Array formulas for dynamic ranges
- Nested IF statements with multiple conditions
- LARGE/SMALL for ranking

**Solver Optimization:**
- Objective function setup
- Constraint configuration
- Scenario comparison
- Sensitivity analysis

**Dashboard Design:**
- Conditional formatting for performance tiers
- Dynamic charts with named ranges
- Slicer-driven interactivity
- Waterfall charts for variance analysis

---

## 💼 Business Skills Demonstrated

✅ **Financial Analysis:** ROI calculations, budget optimization, variance analysis  
✅ **Strategic Thinking:** Balancing short-term ROI with long-term category presence  
✅ **Stakeholder Management:** Presenting trade-offs to marketing and finance teams  
✅ **Data-Driven Decision Making:** Evidence-based recommendations with scenario analysis  
✅ **Communication:** Executive summaries, detailed methodology, visual storytelling

---

## 🔗 Related Cases

- [E-Commerce P&L Analysis](../ecommerce_pnl/) - Financial modeling and margin analysis
- [Fulfillment Operations](../fulfillment_ops_case/) - Process optimization and KPI tracking

---

## 📚 Key Learnings

1. **80/20 Rule Applies:** Top 20% of campaigns drove 65% of returns
2. **Data Quality Matters:** Spent 40% of time cleaning and validating data
3. **Constraints are Critical:** Strategic requirements sometimes override pure ROI optimization
4. **Visualization Drives Action:** Dashboard made complex analysis accessible to stakeholders
5. **Iterative Approach:** Started with simple ranking, evolved to sophisticated optimization

---

**Author:** Arkaprabha Ray  
**Date:** January 2025  
**Tools:** Excel, Power Query, Solver, Pivot Tables  
**Duration:** 2 weeks analysis + 1 week stakeholder presentation

---

⭐️ **Interested in similar analyses?** Check out my other case studies or connect on [LinkedIn](your-linkedin-url)
