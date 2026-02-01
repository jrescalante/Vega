# Process Capability Histogram - Vega Spec for Deneb

![Process Capability Histogram](https://sentidoanalitica.com/wp-content/uploads/2026/01/process-capability-histogram.gif)

This folder contains a Vega specification designed to be used with Deneb in Power BI:

`process-capability-histogram.json`: Vega specification (English version)

`Process-Capability-Histogram-v1.pbix`: Power BI file with the chart rendered in Deneb

This JSON file is a complete, self-contained Vega spec focused on analytical correctness, statistical logic, and practical business use.

---

## 🚀 What this visual does

This chart allows you to:

* Visualize the **distribution of a process** using a histogram
* Overlay a **fitted normal curve** based on the process mean and variation
* Display **LSL / USL** specification limits
* Identify the **process center (μ)** and spread (σ)
* Evaluate **process capability (Cp, Cpk)**
* Highlight **out-of-spec regions** visually

This visual combines **distribution analysis with statistical capability metrics** to evaluate how well a process meets its specification limits.

---

## 🧠 When to use it

Use this visual when you need to answer:

* Is my process capable of meeting specifications?
* How centered is the process?
* How much natural variation does it have?
* Where are defects or risks located in the distribution?

Applicable to:

* 🏭 Manufacturing (dimensions, weights, tolerances)
* 🧾 Services (lead times, SLA compliance)
* ⚙️ Processes (cycle time, throughput, delays)

---

## 📥 Data requirements

### Required field

| Field       | Type    | Description                   |
| ----------- | ------- | ----------------------------- |
| Measurement | Numeric | Observed value of the process |

### Optional fields

| Field | Type    | Description               |
| ----- | ------- | ------------------------- |
| LSL   | Numeric | Lower Specification Limit |
| USL   | Numeric | Upper Specification Limit |

> ℹ️ If LSL or USL are not provided, the histogram and normal curve will still render, but capability metrics will be disabled.

---

## 📐 Statistical calculations

The following statistics are calculated automatically inside Vega:

* **μ (Mean)**
* **σ (Population standard deviation)**
* **n (Sample size)**

When both specification limits exist:

* **Cp** = (USL − LSL) / (6σ)
* **Cpk** = min((USL − μ)/(3σ), (μ − LSL)/(3σ))

---

## 📊 Visual elements

### Histogram

* Binned distribution of measurements
* Number of bins controlled by a signal (`maxBins`)
* Tooltips include bin range and count

### Normal curve

* Gaussian distribution derived from μ and σ
* Scaled to expected counts per bin
* Used for visual comparison, not hypothesis testing

### Reference & specification lines

* **μ (Mean)** → dashed center line
* **LSL / USL** → dashed red lines (optional)

### Tail shading

* Areas outside specifications are shaded
* Provides immediate visual feedback of nonconforming regions

---

## 🧾 KPIs displayed

Above the chart:

* Sample size (n)
* Mean (μ)
* Standard deviation (σ)
* Cp and Cpk (if specs exist)

These values update dynamically with the data.

---

## ⚙️ Configuration

You may adjust the following signals:

| Signal  | Description                      |
| ------- | -------------------------------- |
| maxBins | Maximum number of histogram bins |

All other calculations are derived automatically.

---

## 🧩 How to use in Deneb (Power BI)

1. Add a **Deneb** visual to your report
2. Choose **Vega** as the specification type
3. Paste the full JSON specification of this visual
4. Map your numeric field to **Measurement**
5. Optionally map **LSL** and **USL**

No additional configuration is required.

---

## ⚠️ Notes & assumptions

* Assumes numeric, continuous data
* Uses population standard deviation (`stdevp`)
* Normal curve is descriptive only
* Designed for interpretability over formal testing
