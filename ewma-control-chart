# EWMA Control Chart (Deneb/Vega)

Exponential Weighted Moving Average (EWMA) control chart implemented in Vega, ready to use in Deneb/Power BI.

---

## 📌 Overview

The EWMA chart is designed to detect small shifts in process mean with higher sensitivity compared to traditional Shewhart charts.  

This visual includes:

- Configurable parameters: `lambda` and `L`.
- Upper and lower control limits (UCL/LCL).
- Shaded band between UCL and LCL.
- Blue line for EWMA values.
- Gray dashed line for the center line (CL).
- Points color-coded by state (*in control* / *out of control*).
- Optional raw measurement crosses for context.
- Tooltips with concise, formatted information.
- Custom legend for points and lines.

---

## 🛠️ Required Dataset

The dataset must contain at least:

| Field       | Description                          |
|-------------|--------------------------------------|
| Observation | Sequential observation number (int). |
| Measurement | Measured value (numeric).            |

Optional fields for parameterization:
- `Lambda`
- `LMultiplier`
- `TargetMean`
- `TargetSigma`

---

## 🔄 Transformation Flow

1. **Data preparation**
   - Convert `Observation` and `Measurement` to numeric.
   - Filter invalid values.
   - Sort ascending by observation.

2. **EWMA calculations**
   - Compute EWMA values.
   - Calculate sigma for EWMA.
   - Derive UCL and LCL.
   - Determine state (*in/out*) based on limits.
   - Add padding for y-domain scaling.

3. **Auxiliary datasets**
   - `last_point`: filters last observation for numeric labels.
   - `legend_items`: static legend entries.

---

## 🎨 Visual Elements

- **Green shaded band** between UCL and LCL.
- **Red dashed lines** for UCL and LCL.
- **Gray dashed line** for CL.
- **Blue line** for EWMA.
- **Symbols**:
  - Circle for *in control*.
  - Diamond for *out of control*.
- **Numeric labels** for last UCL, CL, and LCL.
- **Custom legend** aligned to the right.

---

## 🧾 Tooltips

Each EWMA point displays:

- Observation  
- Measurement  
- EWMA  
- CL  
- UCL  
- LCL  
- Lambda  
- L Multiplier  

All values formatted with two decimals.

---

## 📐 Layout and Signals

- `container`, `viewW`, `viewH`: responsive sizing.
- `padding`: controls legend placement.
- `autosize`: type `fit` for adaptive layout.

---

## 🚀 Usage in Deneb

1. Copy the Vega spec JSON into Deneb.
2. Bind dataset fields:
   - **Observation → Observation**
   - **Measurement → Measurement**
3. Optionally bind parameters (`Lambda`, `LMultiplier`, `TargetMean`, `TargetSigma`).
4. The chart adapts automatically to the container.  
   Interaction is limited to tooltips.

---

## 📊 Example Dataset

```csv
Observation,Measurement
1,9.45
2,7.99
3,9.29
4,11.66
...
30,10.52
