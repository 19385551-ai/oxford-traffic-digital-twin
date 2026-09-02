# 🚦 Oxford Traffic Digital Twin
### An AI-Driven Digital Twin for Urban Traffic Management
**A Case Study of Oxford's Road Network**

---

## 📋 Project Overview

This project presents a prototype **AI-driven digital twin** for urban traffic management, developed as part of an MSc Data Science dissertation at Oxford Brookes University (COMP7039).

The system uses real Oxford road network data to simulate traffic flow and applies a **Random Forest machine learning model** to predict junction-level congestion. The AI-optimised approach demonstrates a **30.2% reduction in average vehicle waiting time** compared to conventional fixed-time signal control across five key Oxford junctions.

---

## 🎯 Key Results

| Metric | Value |
|---|---|
| AI Model Accuracy | **95.24%** |
| Baseline Average Waiting Time | 43.8 seconds |
| AI Optimised Waiting Time | 30.5 seconds |
| Average Improvement | **30.2%** |
| Best Junction (Carfax) | **32.7% improvement** |
| Junctions Covered | 5 |
| Training Records | 840 |

---

## 🗂️ Repository Structure

```
oxford-traffic-digital-twin/
│
├── 📓 Dissertation.ipynb              # Main project notebook (26 cells)
│
├── 📁 Data Files
│   ├── oxford_traffic_full_day.csv    # 840 traffic records (training data)
│   └── results_comparison.csv         # AI vs baseline results
│
├── 🤖 AI Model
│   └── oxford_traffic_model.pkl       # Trained Random Forest model
│
├── 📊 Charts & Visualisations
│   ├── chart1_ai_vs_baseline.png      # AI vs fixed timer comparison
│   ├── chart2_improvement.png         # % improvement per junction
│   ├── chart3_throughput.png          # Traffic throughput by hour
│   ├── chart4_feature_importance.png  # Random Forest feature importance
│   ├── chart5_scenarios.png           # Scenario testing results
│   ├── confusion_matrix.png           # Model evaluation matrix
│   ├── oxford_network_map.png         # Oxford road network visualisation
│   └── oxford_traffic_patterns.png   # Traffic flow patterns
│
├── 🗺️ SUMO Simulation Files
│   ├── oxford_clean.net.xml           # Oxford road network (SUMO format)
│   ├── oxford_headington.net.xml      # Headington test network
│   └── headington_visible.xml         # Routes for car demonstration
│
└── 📸 SUMO_screenshot.png             # Virtual car on Oxford road
```

---

## 🛠️ Technologies Used

| Tool | Version | Purpose |
|---|---|---|
| SUMO | 1.27.1 | Traffic simulation (digital twin) |
| Python | 3.12 | Primary development language |
| OSMnx | 2.1.1 | Oxford road network extraction |
| scikit-learn | 1.6.1 | Random Forest AI model |
| pandas | 2.2.2 | Data processing |
| numpy | 2.0.2 | Numerical computation |
| matplotlib | 3.9.0 | Visualisation |
| Google Colab | — | Development environment |

**All tools are free and open-source — no licensing costs required.**

---

## 📦 Requirements

### Python Libraries
```bash
pip install osmnx scikit-learn matplotlib pandas numpy folium
```

### SUMO (for simulation)
Download from: https://sumo.dlr.de/docs/Downloads.php
- Version: 1.27.1 or later
- XQuartz required on Mac for GUI

---

## 🚀 How to Run

### Option 1 — Run the Full Project in Google Colab (Recommended)

1. Open **[Dissertation.ipynb](./Dissertation.ipynb)** in Google Colab
2. Click **Runtime → Run All**
3. All results will be generated automatically

The notebook runs through 26 cells covering:
- Oxford road network download and visualisation
- Traffic data generation and exploration
- AI model training and evaluation
- Results comparison and charts
- Scenario testing

### Option 2 — Run Locally

```bash
# Clone the repository
git clone https://github.com/19385551-ai/oxford-traffic-digital-twin

# Install dependencies
pip install osmnx scikit-learn matplotlib pandas numpy

# Open Jupyter notebook
jupyter notebook Dissertation.ipynb
```

### Option 3 — Run SUMO Simulation (Requires SUMO installed)

```bash
# Set SUMO_HOME (Mac)
export SUMO_HOME="/Library/Frameworks/EclipseSUMO.framework/Versions/1.27.1/EclipseSUMO/share/sumo"

# Run simulation with GUI
sumo-gui -c headington_visible.xml

# Run simulation headless
sumo -c oxford_clean.net.xml
```

---

## 🗺️ Study Area

**Location:** Central Oxford, United Kingdom
**Centre Point:** Carfax Tower (51.7520°N, 1.2577°W)
**Radius:** 1,000 metres
**Network:** 264 junctions, 589 road segments

### Five Monitored Junctions

| Junction | Baseline Wait | AI Wait | Improvement |
|---|---|---|---|
| Carfax | 52.3s | 35.2s | **32.7%** |
| St Giles | 48.1s | 33.7s | **29.9%** |
| Headington | 44.7s | 31.5s | **29.5%** |
| Botley Road | 38.2s | 26.9s | **29.6%** |
| Iffley Road | 35.9s | 25.4s | **29.2%** |

---

## 🤖 AI Model Details

- **Model Type:** Random Forest Classifier
- **Algorithm:** scikit-learn RandomForestClassifier
- **Trees:** 100
- **Max Depth:** 10
- **Train/Test Split:** 70% / 30%
- **Accuracy:** 95.24%

### Input Features
| Feature | Importance |
|---|---|
| Queue Length | 0.38 |
| Waiting Time | 0.28 |
| Vehicle Count | 0.18 |
| Hour of Day | 0.09 |
| Junction | 0.05 |
| Minute | 0.02 |

### Load and Use the Model
```python
import pickle
import pandas as pd

# Load model
with open('oxford_traffic_model.pkl', 'rb') as f:
    model = pickle.load(f)

# Predict congestion
sample = pd.DataFrame([{
    'junction_encoded': 1,  # 0=Botley, 1=Carfax, 2=Headington, 3=Iffley, 4=St_Giles
    'hour': 8,
    'minute': 30,
    'vehicles_count': 15,
    'queue_length': 20,
    'waiting_time': 65.0
}])

prediction = model.predict(sample)
probability = model.predict_proba(sample)[0][1]

print(f"Congested: {'Yes' if prediction[0] else 'No'}")
print(f"Probability: {probability*100:.1f}%")
```

---

## 📊 Scenario Testing

Two planning scenarios were tested to demonstrate the digital twin's value as a decision-support tool:

### Scenario 1 — Road Closure (Cornmarket Street)
- Simulated 47% traffic volume increase at Carfax
- AI predicted congestion with **94% probability**
- Queue length increased from 12.6 to 18.5 vehicles

### Scenario 2 — Demand Surge (Large Event)
- Simulated 80% increase across all junctions
- AI predicted congestion at **all 5 junctions**
- Demonstrates network-wide early warning capability

---

## 📂 Data Sources

| Source | Data | Licence |
|---|---|---|
| [OpenStreetMap](https://www.openstreetmap.org) | Road network geometry | ODbL |
| [DfT Road Traffic Statistics](https://roadtraffic.dft.gov.uk/downloads) | Traffic counts (AADF) | OGL v3 |

**No personal or sensitive data was collected or used.**

---

## ⚠️ Known Limitations

1. **Synthetic training data** — AI trained on Poisson-modelled data, not live Oxford traffic
2. **Geographic scope** — Limited to 5 junctions within 1km of Carfax Tower
3. **No real-time integration** — AI operates as offline predictor, not live signal controller
4. **Vehicle routing** — SUMO routing validation limited by Oxford's pedestrian zones

---

## 🔮 Future Work

- Integration with real-time DfT/HERE Traffic data streams
- Real-time signal control via SUMO TraCI API
- Extension to Oxford's full road network
- Reinforcement Learning (DQN) agent for signal optimisation

---

## 👤 Author

**Chaitra Thimmaiah**
- Student Number: 19385551
- Course: MSc Data Science
- Module: COMP7039 — Dissertation in Computing Subjects
- University: Oxford Brookes University
- Supervisor: Muhammad Younas
- Submission Date: 26 September 2026

---

## 📄 Licence

This project is released under the **MIT Licence** — free to use, modify, and reproduce with attribution.

---

## 📚 Key References

- Adarbah et al. (2024) — BIRCH-based digital twin traffic management
- Lopez et al. (2018) — SUMO microscopic simulation
- Yau et al. (2017) — Reinforcement learning for traffic signal control
- Boeing (2017) — OSMnx road network extraction
- Ma et al. (2020) — Traffic simulation calibration

*Full reference list available in the dissertation report.*
