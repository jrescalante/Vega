# Population Pyramid - Vega Spec for Deneb

![Population Pyramid](https://sentidoanalitica.com/wp-content/uploads/2026/07/population-pyramid.png)

This folder contains a Vega specification designed to be used with Deneb in Power BI:

`population-pyramid.json`: Vega specification (English version)

`Population-pyramid-v1.pbix`: Power BI file with the chart rendered in Deneb

This JSON file is a complete, self-contained Vega spec focused on analytical clarity, comparison logic, and practical reporting use.

---

## 🚀 What this visual does

This visual is a population pyramid designed to compare the distribution of people by age and sex across one or two selected years.

It allows you to:

* Visualize the population structure by age band and sex
* Compare the latest visible year against a second historical year when available
* Show male values on the left and female values on the right
* Display centered age labels to make the chart easier to read
* Highlight the largest age-band change when two years are selected
* Surface totals, deltas, and percentage changes through KPI cards
* Use tooltips to inspect age, sex, population, and comparison context

The visual is especially useful when you need to understand demographic structure, age concentration, and how the population changes over time.

---

## 📦 What is included

This folder includes everything needed to use the visual in Deneb:

* `population-pyramid.json` - the Vega specification
* `Population-pyramid-v1.pbix` - an example Power BI file

---

## 📥 Data requirements

The visual expects four fields from the model:

| Field | Type | Description |
| ----- | ---- | ----------- |
| year | Numeric | Year of the record. |
| age | Numeric | Age of the person or population group. |
| sex | Numeric | Sex code used by the model. The current spec maps `1` to male and `0` to female. |
| people | Numeric | Population count for the row. |

> ℹ️ The visual works best when the data is already clean and each row represents a year, age, sex, and population value.

---

## 📐 Automatic calculations

The visual performs the following calculations inside Vega:

* Converts the incoming fields to numeric values
* Groups records by year, age, and sex
* Keeps the latest visible year as the current view
* Uses a second selected year as the comparison view when available
* Aggregates totals by sex and age band
* Computes current totals, comparison totals, absolute variation, and relative variation
* Identifies the age band with the largest change when comparing two years
* Formats KPI values and tooltip values dynamically

No extra DAX measures or Power Query transformations are required for the base logic.

---

## 📊 Visual elements

### Population bars

* Male values are drawn to the left of the center line
* Female values are drawn to the right of the center line
* Bars are offset slightly to keep the central labels readable
* The active year is rendered as the main population shape

### Comparison layer

* When a second year is selected, a comparison layer appears on top of the main pyramid
* This layer highlights the relative change between the current year and the comparison year
* It helps identify where the population structure grew or shrank

### Centered age labels

* Age labels are shown in the middle of the chart
* Ages are grouped into five-year bands
* This improves readability and keeps the chart symmetric

### KPI cards

Above the chart, the visual shows:

* Current population total
* Comparison population total
* Absolute variation
* Relative variation
* Age band with the largest change

These indicators update automatically with the selected filter context.

### Tooltips

The tooltip shows:

* Age
* Sex
* Population
* Percentage of sex total
* Active year or comparison year context

When comparison is active, the tooltip also exposes the relative variation.

---

## 🧭 How to read the chart

The chart is built to support a fast demographic reading:

* The center axis represents age bands
* The left side represents male population
* The right side represents female population
* Bar length represents the population count
* KPI cards summarize the overall change in the selected context

This makes the visual suitable for comparing population structure across years, regions, or segments.

---

## 🧩 How to use it in Deneb

1. Add a **Deneb** visual to your Power BI report.
2. Choose **Vega** as the specification type.
3. Paste the full contents of `population-pyramid.json`.
4. Map your fields to `year`, `age`, `sex`, and `people`.
5. Optionally filter the report to one or two years to activate the comparison view.

No additional configuration is required beyond the field mapping.

---

## 💡 Best use cases

This visual is useful for:

* Demographic analysis
* Population structure reporting
* Public sector dashboards
* Health and social planning
* Territory analysis
* Age-band comparison over time
* Sex distribution analysis

---

## ⚠️ Notes and assumptions

* Assumes numeric year, age, sex, and population inputs
* Uses 1 for male and 0 for female in the current mapping
* Comparison view appears only when two years are available in the filter context
* Designed for readability and balanced visual comparison rather than decorative styling

---

## 📌 Author

Jose Rafael Escalante

LinkedIn: https://www.linkedin.com/in/jrescalante/

Email: jrescalante85@gmail.com
