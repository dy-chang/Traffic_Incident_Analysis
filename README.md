# Traffic Incident Clearance Time Prediction (MDOT CHART)

## Overview

This project develops a data-driven framework to analyze and predict **traffic incident clearance time** using multi-year operational event data from the Maryland Department of Transportation (MDOT CHART system).

The goal is to support traffic operations and incident management by identifying:
- how long incidents are likely to last
- which incidents are likely to become long-duration events
- key operational and contextual factors driving incident duration

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

### 1. Data Processing

- Cleaned and standardized multi-year event logs
- Parsed location text into structured features:
  - route (e.g., I-95, MD-295)
  - direction (NB/SB/EB/WB)
- Generated time-based features:
  - hour, day of week, month
  - peak vs off-peak indicators

### 2. Target Variables

- **Regression target**
  - log(duration in minutes)

- **Classification target**
  - long-duration incident (> 60 minutes)

---

## Modeling

### Regression Model (Duration Prediction)

- Model: Random Forest Regressor
- Input features:
  - event type
  - route and direction
  - time of day
  - operations center

#### Results

| Model | MAE (log) | R² |
|------|----------|----|
| Baseline (median) | 1.21 | ~0 |
| Random Forest | **0.94** | **0.36** |

#### Interpreted in minutes

- Median absolute error: **~8 minutes**
- Mean absolute error: **~26 minutes**

---

### Classification Model (Long Duration > 60 min)

- Model: Random Forest Classifier
- Class imbalance handled via `class_weight="balanced"`

#### Results

| Model | Accuracy | Recall | AUC |
|------|----------|--------|-----|
| Baseline | 0.86 | 0.00 | - |
| Random Forest | **0.75** | **0.76** | **0.83** |

#### Interpretation

- Baseline model fails to detect long incidents (recall = 0)
- Proposed model successfully identifies **~76% of long-duration incidents**
- AUC = 0.83 indicates strong predictive performance

---

## Key Insights

- **Event type is the strongest predictor of incident duration**
- **Time-of-day patterns significantly affect clearance time**
- Certain routes and operational centers show systematically longer durations
- Long-duration incidents can be identified with high recall despite class imbalance

---

## Outputs

### Figures

- Predicted vs actual duration
- Feature importance (regression and classification)

### Tables

- Regression metrics
- Classification metrics
- Event-type duration statistics

---

Focus: Traffic modeling, incident analysis, resilience, and AI-driven transportation systems

## Project Structure

traffic-incident-clearance-time-prediction/
│
├── notebooks/
│ ├── 01_data_inventory.ipynb
│ ├── 02_chart_cleaning.ipynb
│ └── 03_chart_modeling.ipynb
│
├── outputs/
│ ├── figures/
│ └── tables/
│
├── data/
│ └── processed/
│
└── README.md

---

## Skills Demonstrated

- Large-scale data processing (multi-year, multi-source datasets)
- Feature engineering from semi-structured text (location parsing)
- Predictive modeling (regression + classification)
- Handling imbalanced classification problems
- Model evaluation and interpretation
- Transportation operations analytics

---

## Applications

This framework can support:

- Traffic Management Centers (TMCs)
- Incident response planning
- Traveler information systems
- Real-time congestion management

---

## Next Steps

- Integrate detector data (traffic state before incidents)
- Incorporate crash report features (vehicle and severity)
- Develop real-time prediction framework
- Extend to spatial network-level analysis

---

## Author

Transportation Data Scientist / Modeler  
Focus: Traffic modeling, incident analysis, resilience, and AI-driven transportation systems
