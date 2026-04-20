# Traffic Incident Clearance Time Prediction (MDOT CHART)

## Overview

This project develops a data-driven framework to analyze and predict **traffic incident clearance time** using multi-year operational event data from the Maryland Department of Transportation (MDOT CHART system).

The goal is to support traffic operations and incident management by identifying:

- how long incidents are likely to last  
- which incidents are likely to become long-duration events  
- key operational and contextual factors driving incident duration  

---

## Key Results

- R² = **0.36** for duration prediction  
- Median error ≈ **8 minutes**  
- Mean absolute error ≈ **26 minutes**  
- AUC = **0.83** for identifying incidents lasting more than 60 minutes  
- Recall = **0.76** for long-duration incidents (vs baseline = 0)  

---

## Data

### Primary Dataset

- **MDOT CHART Event Logs (2020–2024)**
- ~100,000 events per year (~500,000+ total records)

### Key Fields

- Event type (collision, disabled vehicle, obstruction, etc.)
- Start time / closed time
- Location (route, direction, exit)
- Operations center
- Duration (incident clearance time)

> Raw data are not included due to size and access restrictions.  
> This repository contains reproducible processing, modeling, and results.

---

## Methodology

### Data Processing

- Cleaned and standardized multi-year event logs  
- Parsed location text into structured features:
  - route (e.g., I-95, MD-295)
  - direction (NB/SB/EB/WB)
- Generated time-based features:
  - hour, day of week, month
  - peak vs off-peak indicators  

---

## Modeling

### Regression Model (Duration Prediction)

- Model: Random Forest Regressor  
- Target: log(duration in minutes)

#### Performance

| Model | MAE (log) | R² |
|------|----------|----|
| Baseline (median) | 1.21 | ~0 |
| Random Forest | **0.94** | **0.36** |

#### Interpretation

- Median error ≈ **8 minutes**
- Strong improvement over baseline
- Captures key temporal and operational patterns

---

### Classification Model (Long Duration > 60 min)

- Model: Random Forest Classifier  
- Class imbalance handled via `class_weight="balanced"`

#### Performance

| Model | Accuracy | Recall | AUC |
|------|----------|--------|-----|
| Baseline | 0.86 | 0.00 | - |
| Random Forest | **0.75** | **0.76** | **0.83** |

#### Interpretation

- Baseline fails to detect long incidents
- Model identifies ~76% of long-duration incidents
- Strong predictive power (AUC = 0.83)

---

## Key Insights

- **Event type is the strongest predictor of incident duration**
- **Time-of-day significantly impacts clearance time**
- Certain routes and operational centers exhibit longer durations
- Long-duration incidents can be reliably identified despite class imbalance

---

## Project Structure

```text
traffic-incident-clearance-time-prediction/
│
├── notebooks/
│   ├── 01_data_inventory.ipynb
│   ├── 02_chart_cleaning.ipynb
│   └── 03_chart_modeling.ipynb
│
├── outputs/
│   ├── figures/
│   └── tables/
│
├── data/
│   └── processed/
│
└── README.md
