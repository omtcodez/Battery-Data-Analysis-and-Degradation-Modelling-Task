
# 🔋 Battery Data Analysis & Degradation Modeling

Welcome! This project explores how lithium-ion batteries age over time by analyzing real-world battery cycle data. From basic data cleaning to modeling battery degradation using machine learning, this notebook walks through the full process.

---

## 📘 Overview

Battery degradation is critical to understand — especially for electric vehicles, mobile devices, and renewable energy storage. In this case study, we're provided with two datasets from battery cycling experiments and asked to perform data analysis, compute metrics like **State of Health (SoH)**, and model how battery capacity changes over time.

---

## 📂 What’s in This Project?

| File                             | Description |
|----------------------------------|-------------|
| `Battery_Analysis_Complete.ipynb` | Main notebook with all analysis, charts, and code |
| `LR1865SZ_cycles201214_002_4.xlsx` | Dataset containing early cycles |
| `LR1865SZ_cycles201217_001_2.xlsx` | Dataset with additional cycles |
| `README.md`                      | This file you're reading now 😊 |

---

## 🧠 Step-by-Step Thought Process

### 1. **Understanding the Data**
Before coding, I studied the column descriptions. These include voltage, current, capacity, energy, and timestamps per second. One full **charge + discharge = 1 cycle**, and each row represents a second-by-second measurement.

**Objective:** Understand the battery's behavior cycle by cycle.

---

### 2. **Data Loading & Merging**
The two Excel files were imported using `pandas.read_excel()` and combined using `pd.concat()`.

**Why?** The test is continuous over multiple files. Combining them ensures analysis spans the full battery lifecycle.

---

### 3. **Cleaning the Data**
I removed duplicates, handled missing values, and ensured time was in seconds using `Date_Time`. This was especially important for later calculations of energy and capacity over time.

**Why?** Battery analysis is sensitive to time consistency — even small gaps can distort capacity calculations.

---

### 4. **Exploratory Data Analysis (EDA)**
I plotted histograms and scatter plots for:
- Voltage distribution
- Current values
- Capacity vs. Voltage
- Energy vs. Cycle

**Why?** To get a feel for how the battery performs and spot any anomalies (like sudden drops in voltage or current spikes).

---

### 5. **Splitting into Charging and Discharging Phases**
By checking the sign of current:
- `Current > 0` → Charging
- `Current < 0` → Discharging

This let us analyze the two phases separately — important because batteries behave differently while charging vs. discharging.

---

### 6. **Calculating Capacity per Cycle**
Using the trapezoidal integration formula:
```
Capacity(k+1) = Capacity(k) + Current(k+1) * ΔTime / 3600
```
I grouped data by `Cycle_Index` and integrated current over time to get capacity in Ampere-hours (Ah).

**Why?** Manufacturers rate batteries in Ah. Tracking how this drops over cycles shows degradation.

---

### 7. **Computing C-rate**
C-rate = Current / Rated Capacity  
This shows how "hard" the battery is being used.

I binned C-rates into categories:
- Low (≤0.5C)
- Medium (≤1C)
- High (≤2C)
- Very High (>2C)

**Why?** High C-rates can speed up degradation. Categorizing them lets us compare how usage intensity affects battery life.

---

### 8. **Visualizations**
I created charts for:
- Charge capacity vs. Voltage
- Charge capacity vs. Cycle Index
- C-rate vs. Capacity
- State of Health over time

**Why?** Visuals help quickly spot patterns — like declining capacity, sudden drops, or divergence by C-rate.

---

### 9. **Modeling Degradation with Polynomial Regression**
I used a 2nd-degree polynomial regression (`sklearn`) to model capacity vs. cycle number. It fits the gradual curve of degradation well.

Also: I plotted **separate degradation curves per C-rate group** to show how "harder use" accelerates capacity loss.

**Why?** This kind of modeling can help predict when a battery will drop below useful levels — critical for preventive maintenance.

---

### 10. **Calculating State of Health (SoH)**
SoH = Current Cycle Capacity / Initial Capacity  
I plotted this over cycles to show how the battery health declines — from 100% down to ~70% over 100+ cycles.

**Why?** SoH is a key industry metric used to estimate remaining battery life.

---



## 💬 Conclusions

- **Degradation is gradual and follows a predictable curve.**
- **Higher C-rates clearly speed up capacity loss.**
- **Polynomial regression is an effective fit for degradation modeling.**

This project combines clean data science practices with meaningful energy insights — a solid example of real-world battery analytics!

---


