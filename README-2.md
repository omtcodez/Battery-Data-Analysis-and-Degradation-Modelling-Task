
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



## 📋 Detailed Explanations of Case Study Requirements

This section addresses **each task listed in the case study**, providing explanations of what was done, how, and why.

---

### ✅ 1. Import and Join the Datasets

We loaded two battery test datasets in Excel format using `pandas.read_excel()` and joined them using `pd.concat()`.

- This ensures a single continuous dataset containing all cycles.
- Joining them helps perform seamless time-series analysis.

---

### ✅ 2. Analyze Each Variable

We explored:
- **Voltage (V)**: Distribution and relation to capacity.
- **Current (A)**: Sign used to distinguish charging/discharging.
- **Capacity (Ah)**: Calculated over time.
- **Energy (Wh)**: Product of voltage and capacity.
- **Cycle Index**: Main grouping variable for battery cycles.

Each was visualized or statistically described to assess their behavior.

---

### ✅ 3. Perform EDA (Univariate, Bivariate) and Clean the Data

- Used `describe()` for statistical summaries.
- Plotted histograms for current, voltage, energy.
- Visualized relationships (e.g. capacity vs. voltage).
- Cleaned:
  - Missing values in key columns.
  - Duplicates.
  - Time normalization (using `Date_Time` → seconds).

---

### ✅ 4. Extract Charging and Discharging Phases

- Split data based on current sign:
  - Charging phase: `Current > 0`
  - Discharging phase: `Current < 0`

This allows independent analysis of each phase’s capacity and degradation behavior.

---

### ✅ 5. Calculate Charge and Discharge Capacity per Cycle

- Used trapezoidal rule logic:
  	Capacity = ∑ (Current × ΔTime) / 3600
- Grouped by `Cycle_Index`.
- Calculated charge and discharge capacities using `groupby().apply()`.

Why?
- Capacity is a time-integrated metric — this reflects the real energy transferred into/out of the battery.

---

### ✅ 6. Create a New Variable: C-rate

- Formula used:
  	C-rate = |Current| / Nominal Capacity (2.3 Ah)
- Shows how intensely the battery is being charged/discharged.
- Used to analyze its effect on degradation.

---

### ✅ 7. Perform Visualizations

- Charge capacity vs. Voltage
- Charge capacity vs. Cycle Index
- Discharge capacity vs. Cycle Index
- SoH vs. Cycle Index
- C-rate vs. Capacity
- Capacity grouped by C-rate bin

These visuals reveal trends, degradation patterns, and anomalies.

---

### ✅ 8. Influence of C-rate on Charge Capacity + Charge vs. Discharge Differences

**Influence of C-rate:**
- High C-rates (fast charging) → faster capacity degradation.
- Grouped charge capacities by C-rate bins ("Low" to "Very High").
- Higher bins showed **steeper capacity decline** over cycles.

**Charge vs. Discharge Observations:**
- Charge capacity declines smoothly.
- Discharge capacity can fluctuate due to system losses and load variations.
- Overall trend: both decline, but **charging trends are more consistent**.

---

### ✅ 9. ML Modeling: Polynomial Regression

- Used 2nd-degree polynomial regression (`PolynomialFeatures + LinearRegression`) to model capacity vs. cycle number.
- Fit curve shows **gradual degradation**.
- Could be used to **predict future battery health**.

---

### ✅ 10. Calculate SoH and Visualize Degradation

- SoH = (Current cycle charge capacity) / (Initial capacity) × 100
- Shows degradation from 100% to ~70% across ~100 cycles.
- Visualized clearly over `Cycle_Index`.

Why?
- SoH is a real-world metric to assess when a battery should be replaced or maintained.

---

### ✅ 11. Methodology Selection

- Used Pandas for efficient data manipulation.
- Seaborn & Matplotlib for plotting.
- Scikit-learn for modeling degradation.
- Chose polynomial regression to capture non-linear decline typical of batteries.

---

### ✅ 12. Thought Process Behind Each Step

Every step was designed to:
- Mirror real-world battery analysis practices.
- Be explainable and visual.
- Focus on **degradation behavior**, **operational intensity (C-rate)**, and **battery health over time**.

By combining physics-based metrics (capacity, energy) with statistical analysis and machine learning, this project gives a **complete picture of battery aging**.

---
