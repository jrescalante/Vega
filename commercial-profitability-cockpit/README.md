# Commercial Profitability Cockpit - Vega Spec for Deneb

![Commercial Profitability Cockpit](https://sentidoanalitica.com/wp-content/uploads/2026/07/commercial-profitability-cockpit.gif)

## 📌 Description

This project implements a custom **Power BI** visual using **Deneb (Vega)** to analyze commercial profitability against SKU-quarter margin targets.

The current version goes beyond a traditional scatter plot: it compares revenue growth against margin gap, classifies each SKU into commercial quadrants, adds header KPIs, and uses a target table to evaluate each product against its specific margin objective. The result is a visual cockpit for spotting healthy growth, margin risk, value deterioration, and commercial defense opportunities.

---

# 🎯 What does this visual do?

The visual allows you to:

- Compare each SKU's revenue growth against its first visible period.
- Calculate gross margin from revenue, units, and unit cost.
- Evaluate each SKU against its specific quarterly margin target.
- Measure the gap between actual margin and target margin.
- Classify SKUs into commercial profitability and growth quadrants.
- Identify exposed revenue from SKUs below target.
- Read portfolio, profitability, and risk KPIs at the top of the visual.
- Explore details through adaptive Power BI tooltips.
- Respond to slicers, filters, and cross-selection context.

---

# 📦 Repository contents

This repository includes the elements required to use and explore the visual.

### 📄 Vega specification (JSON)

Contains the complete visual definition, including:

- Data transformations.
- Gross margin calculation.
- Revenue growth calculation.
- Margin gap calculation against target.
- Quadrant classification rules.
- Chart layout.
- Tooltips and interactions.
- Header KPIs.
- Semantically colored quadrants with dynamic labels.

### 📊 Power BI file (.pbix)

Includes a working example with sample data, target table, configured measures, and the visual loaded in Deneb.

> ℹ️ File names may vary, but their purpose and structure remain the same.

---

# 📥 Data requirements

The visual works with a fact table at SKU-period grain and a margin target table at SKU-quarter grain. Field names are in English because they are the names expected by the Vega specification included in this project.

## Required fact table fields

| Field | Type | Description |
|--------|------|-------------|
| Date | Date | Analysis period. |
| SKU | Text | Unique product identifier. |
| Revenue | Numeric | Period revenue. |
| Units_Sold | Numeric | Units sold in the period. |
| Unit_Cost | Numeric | Unit cost used to calculate gross margin. |
| SKUQuarterKey | Text | SKU-quarter composite key, for example `SKU-001|2024Q1`. |

## Recommended fact table fields

| Field | Type | Description |
|--------|------|-------------|
| Product | Text | Descriptive product name. |
| **Category** | Text | Commercial product category. Recommended for slicing the portfolio and improving tooltip context. |
| **Channel** | Text | Sales channel. Recommended for analyzing margin pressure by route-to-market or commercial channel. |
| Unit_Price | Numeric | Unit selling price. |
| Year | Numeric | Calendar year used, together with `QuarterNo`, to build `QuarterKey`. |
| QuarterNo | Numeric | Quarter number used, together with `Year`, to build `QuarterKey`. |
| Quarter | Text | Quarter label, for example `Q1`. |
| QuarterKey | Text | Year-quarter key, for example `2024Q1`. |

> ℹ️ `Product`, `Category`, and `Channel` are optional for the visual to work, but `Category` and `Channel` are especially useful for slicing profitability patterns. `QuarterKey` is derived from `Year` and `QuarterNo`, then combined with `SKU` to create `SKUQuarterKey`.

## Margin target table

The `Margin Targets` table contains the margin objective by SKU and quarter. This table avoids creating a long calculated DAX table and keeps targets as a business input.

| Field | Type | Description |
|--------|------|-------------|
| SKU | Text | Product identifier used for readability and audit checks. |
| Year | Numeric | Target year. |
| Quarter | Text | Target quarter, for example `Q1`. |
| Quarter Key | Text | Year-quarter key, for example `2024Q1`. |
| SKU Quarter Key | Text | Relationship key to `Data_EN[SKUQuarterKey]`. |
| SKU-Q Target Margin | Numeric | Target margin for the SKU-quarter. |

Expected relationship:

```text
Data_EN[SKUQuarterKey] -> Margin Targets[SKU Quarter Key]
```

Every visible `SKUQuarterKey` in the fact table should have a matching row in `Margin Targets`. The measures do not include a default target fallback; if a target is missing, the result should return blank so the data coverage issue remains visible.

---

# 📐 Required Power BI measures

Unlike visuals that calculate everything directly in Vega, this visual uses two DAX measures to read the target table correctly from the semantic model.

## SKU target margin

Returns the target margin for the current SKU-quarter by looking up the current `SKUQuarterKey` in `Margin Targets`.

```DAX
SKU Target Margin =
VAR CurrentSKUQuarterKey =
    SELECTEDVALUE ( Data_EN[SKUQuarterKey] )
RETURN
    LOOKUPVALUE (
        'Margin Targets'[SKU-Q Target Margin],
        'Margin Targets'[SKU Quarter Key], CurrentSKUQuarterKey
    )
```

## Context target margin

Calculates the revenue-weighted target margin for all SKU-quarters visible in the current filter context.

```DAX
Context Target Margin =
VAR VisibleKeys =
    VALUES ( Data_EN[SKUQuarterKey] )
VAR WeightedTarget =
    SUMX (
        VisibleKeys,
        VAR CurrentKey = Data_EN[SKUQuarterKey]
        VAR RevenueWeight =
            CALCULATE ( SUM ( Data_EN[Revenue] ) )
        VAR TargetMargin =
            LOOKUPVALUE (
                'Margin Targets'[SKU-Q Target Margin],
                'Margin Targets'[SKU Quarter Key], CurrentKey
            )
        RETURN
            RevenueWeight * TargetMargin
    )
VAR VisibleRevenue =
    SUMX ( VisibleKeys, CALCULATE ( SUM ( Data_EN[Revenue] ) ) )
RETURN
    DIVIDE ( WeightedTarget, VisibleRevenue )
```

These two measures must be added to the Deneb visual together with the required fields. `SKU Target Margin` classifies each point, while `Context Target Margin` provides the weighted reference target for the visible filter context.

---

# 📐 Automatic calculations

Once the fields and measures are available in Deneb, Vega automatically calculates:

- Row-level gross margin.
- Gross margin percentage.
- Revenue growth against the first visible period.
- Alternative growth against the portfolio mean when only one historical point exists.
- Margin gap against the SKU-quarter target.
- Commercial quadrant classification.
- Profitable and critical SKUs.
- Exposed revenue from SKUs below target.
- Visible weighted gross margin.
- Visible category and product counts.

---

# 📊 Commercial classification

Each SKU is classified using two axes: revenue growth and margin gap against target.

| Quadrant | Condition | Interpretation |
|-----------|-----------|----------------|
| Defend Volume | Negative growth and margin above target | Margin holds while revenue declines. |
| Grow and Profitable | Positive growth and margin above target | Healthy growth with target profitability. |
| Value Destruction | Negative growth and margin below target | Revenue and margin deteriorate at the same time. |
| Growth with Margin Risk | Positive growth and margin below target | Growth is happening, but profitability is under pressure. |

The horizontal line represents `On Target`: a margin gap equal to zero. Above that line, the SKU is above its target; below it, the SKU is below target.

---

# 📊 Visual elements

The visual combines several components to support both executive and analytical profitability reading.

Includes:

- **Main scatter plot**: shows each SKU by revenue growth and margin gap against target. It helps detect concentration, dispersion, and critical points.
- **Commercial quadrants**: translate numeric results into actionable business reading: defend volume, grow profitably, destroy value, or grow with margin risk.
- **Reference lines**: mark zero revenue growth and zero margin gap against target. They provide the baseline for interpreting each point.
- **Quadrant colors**: assign a visual family to each commercial condition. They speed up the reading of each SKU state.
- **Revenue-based point size**: gives more visual weight to SKUs with higher visible revenue.
- **Header KPIs**: summarize the visible portfolio by SKUs, profitability, criticality, exposure, and margin.
- **Adaptive tooltips**: show period, SKU, product, category, quadrant, growth, actual margin, target margin, margin gap, and revenue depending on available fields.
- **Power BI filter interaction**: responds to slicers, cross-filters, and category or product selections.

---

# 🧾 Indicators (KPIs)

In addition to the main chart, the visual presents summary indicators at the top to quickly read the state of the visible portfolio.

## Header KPIs

### Visible SKUs

Counts the SKUs represented by the latest visible point for each product.

- Formula: `count(sku_key)` over the latest visible points.
- Calculation base: one point per SKU after applying the filter context.

### Profitable SKUs

Shows the share of visible SKUs that are at or above their margin target.

- Formula: `profitable_sku_count / sku_count`
- Calculation base: `profitable_sku_count` = SKUs with margin gap greater than or equal to zero.

### Critical SKUs

Shows the share of visible SKUs that are below their margin target.

- Formula: `critical_sku_count / sku_count`
- Calculation base: `critical_sku_count` = SKUs with negative margin gap.

### Exposed revenue

Measures the share of visible revenue associated with SKUs below target.

- Formula: `exposed_revenue_total / visible_revenue`
- Calculation base: `exposed_revenue_total` = revenue from SKUs with negative margin gap; `visible_revenue` = total visible revenue.

### Weighted gross margin

Summarizes gross margin for the visible set, weighted by revenue.

- Formula: `gross_margin_total / visible_revenue`
- Calculation base: `gross_margin_total` = revenue minus total cost; `visible_revenue` = total visible revenue.

---

# 💡 Use cases

This visual is especially useful for:

- Revenue Management.
- Commercial profitability control.
- Category Management.
- Margin target tracking.
- Critical SKU prioritization.
- Portfolio management.
- Quarterly commercial planning.
- Profitable growth analysis.
- Exposed revenue identification.
- Conversations across sales, finance, and pricing teams.
