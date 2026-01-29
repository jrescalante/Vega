# SPC Control Chart + WECO Rules – Vega Spec for Deneb

![SPC Control Chart Preview](https://sentidoanalitica.com/wp-content/uploads/2026/01/SPC-Control-Chart-v1.gif)

This folder contains two Vega specifications designed to be used with **Deneb in Power BI**:

- `spc-chart-en.json`: English version  
- `spc-chart-es.json`: Spanish version  
- `SPC-Control-Chart-v1.pbix`: Power BI file with both charts rendered in Deneb

Each JSON file is a complete, self-contained Vega spec focused on analytical correctness, statistical rigor, and practical business use.

This implementation builds an **SPC (Shewhart) Control Chart** with automated statistical calculations and rule detection.

---

## 📊 What this visual does

This Vega specification builds a full Statistical Process Control chart directly inside Deneb, including:

* Automatic calculation of **Center Line (CL)** and **σ (population standard deviation)**
* Control limits: **UCL / LCL (±3σ)**
* Zone visualization:
  * ±1σ (in control)
  * ±2σ (warning)
  * ±3σ (out of control)
* **WECO rules (1–8)** detection using rolling windows
* Optional **USL / LSL** support
* **Cpk calculation** (only shown when both USL and LSL exist)
* KPI cards and rule summary grid
* Accessible color palette (Okabe–Ito)

All calculations are performed inside Vega — no DAX measures required.

---

## 🧩 Data requirements

Your dataset must contain at least the following fields:

| Field         | Description                                  |
| ------------- | -------------------------------------------- |
| `Observation` | Sequential index (numeric or numeric string) |
| `Measurement` | Measured value                               |

Optional fields:

| Field | Description               |
| ----- | ------------------------- |
| `usl` | Upper Specification Limit |
| `lsl` | Lower Specification Limit |

⚠️ **Important:** Column names must match exactly as defined above. They are **case sensitive**, so for example `Measurement` is valid but `measurement` will not be recognized.

If `usl` and `lsl` are not provided, specification-related KPIs (USL, LSL, Cpk) are automatically hidden.

---

## 🧠 Statistical logic

* **CL** = mean(Measurement)
* **σ** = population standard deviation (stdevp)
* **UCL / LCL** = CL ± 3σ
* **Zones** are derived from σ offsets
* **WECO rules** are evaluated per observation using rolling windows over z-scores, signs, and trends

The visual highlights:

* Out-of-control points
* Warning states
* Runs and trends

---

## 🖱 Interaction

* Tooltips only (no brushing or cross-filtering)
* Hover highlights points and rule-related conditions
* Designed to be lightweight and stable inside Power BI

---

## 🚀 How to use in Deneb

1. Add **Deneb** to your Power BI report
2. Choose **Vega** as the specification type
3. Paste the full JSON code from this file
4. Bind your dataset with the required fields
5. (Optional) Add `usl` / `lsl` columns to enable specification KPIs

---

## 🎯 Design goals

* Keep all statistical logic **inside the visual layer**
* Avoid unnecessary DAX complexity
* Provide explainable, auditable SPC behavior
* Make the spec reusable across datasets and reports

---

## 📌 Notes

* Each JSON specification includes its own **locale configuration**:
  * `spc-control-chart-en.json` → uses **US locale** (dot for decimals, comma for thousands)
  * `spc-control-chart-es.json` → uses **Spanish locale** (comma for decimals, dot for thousands, localized labels)
* Column names in your dataset must match the expected names (`Observation`, `Measurement`, `usl`, `lsl`) and are **case sensitive**.
* Tested with Deneb inside Power BI Desktop
* Intended for educational, analytical, and production scenarios
* The `.pbix` file included in this folder contains both Vega specs rendered in Deneb for immediate use or reference
