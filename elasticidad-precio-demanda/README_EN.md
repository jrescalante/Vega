# Price Elasticity Scatter – Vega Spec for Deneb

![Elasticidad Precio-Demanda](https://sentidoanalitica.com/wp-content/uploads/2026/06/elasticidad-precio-demanda-v2.gif)

## 📌 Description

This project implements a custom **Power BI** visual using **Deneb (Vega)** to analyze **price elasticity of demand** for products.

The current version goes beyond a classic scatter plot: it automatically calculates elasticity, classifies commercial behavior, organizes the reading by quadrants, and adds temporal traceability when a single product is filtered. It also includes adaptive tooltips, top KPIs, and an optional trend line to support interpretation.

---

# 🎯 What does this visual do?

The visual allows you to:

- Automatically calculate price elasticity of demand.
- Classify products according to price sensitivity.
- Identify elastic, inelastic, and atypical behavior.
- Analyze the impact of price changes on sales.
- Explore information through tooltips and Power BI interaction.
- Read the chart by quadrants with a clear commercial meaning.
- Show temporal traceability and point numbering when a single product is selected.
- Activate a trend line only when there are enough points for it to be interpretable.

---

# 📦 Repository Contents

This repository includes everything needed to use and explore the visual.

### 📄 Vega Specification (JSON)

Contains the full definition of the visual, including:

- Data transformations.
- Statistical calculations.
- Classification rules.
- Chart design.
- Tooltips and interactions.
- Quadrants with semantic colors and dynamic labels.
- Traceability by filtered product.
- Conditional trend line.

### 📊 Power BI File (.pbix)

Includes a fully functional example with sample data and the visual configured in Deneb.

> ℹ️ File names may vary, but their purpose and structure remain the same.

---

# 📥 Data Requirements

The visual works with five required fields to calculate elasticity and can be enriched with two optional fields to improve labels and tooltips.

## Required Fields

| Field | Type | Description |
|--------|------|-------------|
| Date | Date | Analysis period. |
| SKU | Text | Unique product identifier. |
| Unit_Price | Numeric | Product selling price. |
| Units_Sold | Numeric | Quantity sold in each period. |
| Revenue | Numeric | Total sales for the period. |

## Optional Fields

| Field | Type | Description |
|--------|------|-------------|
| Product | Text | Descriptive product name. |
| Category | Text | Product commercial category. |

> ℹ️ The visual works correctly using only the required fields. Optional fields are used to improve the information shown in labels, quadrants, and tooltips.

---

# 📐 Automatic Calculations

All calculations are executed directly inside Vega. There is no need to create DAX measures, calculated columns, or additional Power Query transformations.

The visual automatically calculates:

- Price percentage change.
- Units sold percentage change.
- Price-demand elasticity.
- Automatic elasticity classification.
- Revenue change.
- Price trend.
- Demand trend.
- Temporal traceability by filtered product.
- Linear regression reference when the sample is sufficient.

---

# 📊 Elasticity Classification

Each observation is automatically classified according to the calculated value.

| Elasticity | Interpretation |
|--------------|---------------|
| Greater than 1 | Elastic demand |
| Between 0 and 1 | Inelastic demand |
| Equal to 1 | Unit elasticity |
| Less than 0 | Atypical behavior |

---

# 📊 Visual Elements

The visual combines different components to support a technical and commercial reading of price elasticity of demand.

It includes:

- **Main scatter**: shows each observation in the price-demand plane. Identifies elasticity patterns, concentration, and dispersion.
- **Commercial quadrants**: separate behavior into promotional impulse, value capture, mixed deterioration, and volume risk. They translate the technical point into an actionable business reading.
- **Reference lines**: mark the axis crossing and the overall average. They serve as a comparison baseline for each observation.
- **Classification colors**: assign a visual family according to elastic, unitary, inelastic, or atypical behavior. They speed up interpretation.
- **Smart labels**: adjust quadrant text according to the visible context. They improve readability without cluttering the view.
- **Adaptive tooltips**: change the level of detail according to the active filter. They show fewer or more fields depending on whether a product, a category, or the full portfolio is being analyzed.
- **Product traceability**: orders the points by date when a single SKU is filtered. It allows the temporal evolution of the product to be followed inside the scatter.
- **Conditional trend line**: appears only when there are enough points and a valid regression. It summarizes the general direction of the behavior without competing with the historical trace.
- **Power BI filter interaction**: responds to slicers and cross-filters. It allows the same visual to be analyzed from different portfolio views.

---

# 🧾 Indicators (KPIs)

In addition to the main chart, the visual presents summarized indicators at the top that provide an overall view of dataset behavior.

## Top KPIs

These indicators are automatically calculated from the latest visible filter state and are easier to read as grouped blocks:

### Sensitive portfolio

Measures how much of the visible portfolio behaves with high elasticity.

- Formula: `elastic_sku_count / sku_count`
- Calculation basis: `sku_count` = SKUs visible in the current context and `elastic_sku_count` = SKUs whose latest point is classified as elastic.

### Volume risk and value capture

These two KPIs show the commercial distribution of visible SKUs according to their latest point in the scatter.

- Volume risk: `risk_sku_count / sku_count`
- Value capture: `capture_sku_count / sku_count`
- Calculation basis: `risk_sku_count` = SKUs whose latest point falls into price up and demand down; `capture_sku_count` = SKUs whose latest point falls into price up and demand up.

### Exposed revenue

Indicates what share of visible revenue is associated with SKUs that carry risk or high sensitivity.

- Formula: `exposed_ingresos / visible_ingresos`
- Calculation basis: `exposed_ingresos` = revenue from SKUs exposed to volume risk or high elasticity; `visible_ingresos` = total visible revenue.

### Weighted elasticity

Summarizes portfolio elasticity while giving more weight to SKUs with higher revenue.

- Formula: `weighted_elasticidad_sum / valid_elasticidad_ingresos_sum`
- Calculation basis: `weighted_elasticidad_sum` = sum of elasticity weighted by revenue; `valid_elasticidad_ingresos_sum` = revenue only from rows with valid elasticity.

---

# 💡 Use Cases

This visual is especially useful for:

- Pricing analysis.
- Revenue Management.
- Strategic pricing.
- Retail.
- Consumer goods.
- Manufacturing.
- Commercial Intelligence.
- Category Management.
