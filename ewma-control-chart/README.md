# EWMA Control Chart – Vega Spec for Deneb
![EWMA Control Chart](https://sentidoanalitica.com/wp-content/uploads/2026/01/EWMA-Control-Chart-v1.gif)

This folder contains a Vega specification designed to be used with **Deneb in Power BI**:

- `ewma-control-chart.json`: English version
- `EWMA-Control-Chart-v1.pbix`: Power BI file with the chart rendered in Deneb

Each JSON file is a complete, self-contained Vega spec focused on analytical correctness, statistical logic, and practical business use.

---

## 📊 What this visual does

This Vega specification builds an **Exponentially Weighted Moving Average (EWMA) control chart** directly inside Deneb, including:

* Exponentially weighted moving average (EWMA) calculation per observation
* Target Mean and Target Sigma input signals
* Configurable **λ (lambda)** and **L Multiplier**
* Dynamic control limits updating with λ
* Clear **in-control / out-of-control states**
* Primary limits visualized as shaded boundaries
* Data point coloring based on control evaluation

The visual is designed to detect **small shifts in process mean** more effectively than traditional charts. :contentReference[oaicite:1]{index=1}

---

## 🧩 Data requirements

Your dataset must contain at least the following fields:

| Field       | Description                        |
|-------------|------------------------------------|
| `Observation` | Sequential index (numeric)       |
| `Value`       | Measured data point             |

⚠️ **Important:** Column names are **case sensitive**.

---

## 🧠 Statistical logic

* **EWMA** = exponential smoothing over observations
* **Control limits** adapt based on λ and L multiplier
* Out-of-control detection is driven by EWMA crossing dynamic boundaries
* Limits and signals update interactively via Vega signals

---

## 🖱 Interaction

* Tooltips show observation details and EWMA state
* Hover highlights point and limits
* Signals are exposed at the top of the visual for interactive tuning

---

## 🚀 How to use in Deneb

1. Add **Deneb** to your Power BI report  
2. Choose **Vega** as the specification type  
3. Paste the full JSON code from this file  
4. Bind your dataset with the required fields  
5. Adjust **Target Mean**, **Target Sigma**, **λ**, and **L** as needed

---

## 🎯 Design goals

* Keep all statistical logic **inside the visual layer**
* Avoid unnecessary DAX complexity
* Provide explainable, auditable EWMA behavior
* Make the spec reusable across datasets and reports

---

## 📌 Notes

* You can customize the visual by adjusting the **λ (lambda)** and **L Multiplier** signals
* The primary limits reflect the exponential nature of the chart, not static 3σ boundaries

---

## 👤 Author

**José Rafael Escalante**  
Power BI Consultant & Trainer  
Microsoft MVP – Data Platform  

* GitHub: [https://github.com/jrescalante](https://github.com/jrescalante)  
* LinkedIn: [https://www.linkedin.com/in/jrescalante/](https://www.linkedin.com/in/jrescalante/)

---

## License

This project is shared under the **MIT License**.

Attribution is appreciated when adapting or extending the specification.
