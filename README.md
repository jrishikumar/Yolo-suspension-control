# 🚗 YOLO-Based Predictive Suspension Control Using AI

> Real-time road condition detection using YOLOv8 + adaptive suspension control via MATLAB/Simulink quarter-car model

![Python](https://img.shields.io/badge/Python-3.8+-blue?logo=python)
![MATLAB](https://img.shields.io/badge/MATLAB-Simulink-orange?logo=mathworks)
![YOLOv8](https://img.shields.io/badge/YOLOv8-Ultralytics-purple)
![Status](https://img.shields.io/badge/Status-Completed-green)

---

## 📌 What This Project Does

Most suspension systems are **passive** — they react to a pothole only after the wheel hits it.

This system does something different: it uses a **webcam + YOLOv8** to detect potholes and speed bumps **ahead of the vehicle** and automatically adjusts the suspension damping **before impact** — like giving the car a split-second warning.

The detection output is sent live to a **MATLAB quarter-car simulation**, which adjusts the damping coefficient in real time based on road condition severity.

---

## 🎯 Key Results

| Metric | Value |
|---|---|
| Detection Accuracy | **89%** (high confidence) |
| Vibration Reduction vs Passive | **20–40%** |
| Road Conditions Detected | Pothole, Speed Bump, Crack, Unpaved, Smooth |
| Settling Time | Significantly faster vs passive suspension |
| Model | YOLOv8 trained on Roboflow dataset (100 epochs) |

---

## 🏗️ System Architecture

```
[Webcam Feed]
     ↓
[YOLOv8 Inference] — detects: pothole / speed_bump / crack / unpaved / smooth
     ↓
[Severity Score] — calculated from class weight × confidence × bounding box area
     ↓
[Damping Coefficient Mapping]
  smooth    → cs = 1500 N·s/m
  crack     → cs = 2500 N·s/m
  unpaved   → cs = 3500 N·s/m
  speed_bump→ cs = 4500 N·s/m
  pothole   → cs = 5000 N·s/m
     ↓
[MATLAB Quarter-Car Model] — 2-DOF ODE solved via RK4
     ↓
[Live Output] — body displacement, acceleration, suspension deflection
```

---

## 🛠️ Tools & Technologies

| Tool | Purpose |
|---|---|
| YOLOv8 (Ultralytics) | Real-time object detection |
| Python (OpenCV, socket, json) | Detection pipeline + TCP communication |
| MATLAB / Simulink | Quarter-car vehicle dynamics simulation |
| Roboflow | Dataset collection and annotation |
| TensorFlow / PyTorch | Model training framework |

---

## 📂 Repository Structure

```
yolo-suspension-control-ev/
├── python/
│   └── yolo_matlab_bridge.py      # Main detection + TCP server script
├── matlab/
│   └── quarter_car_live.m         # MATLAB quarter-car simulation
├── results/
│   ├── pothole_detection.png      # Fig: Detection + adaptive response
│   ├── passive_vs_adaptive.png    # Fig: Performance comparison
│   └── speed_bump_response.png    # Fig: Speed bump test
├── docs/
│   └── project_report.pdf         # Full project report
└── README.md
```

---

## ⚙️ How to Run

### Prerequisites
```bash
pip install ultralytics opencv-python
```
MATLAB R2021a or later with Simulink

### Step 1 — Start the Python YOLO server
```bash
cd python
python yolo_matlab_bridge.py
```
This starts a TCP server on port 65432 and opens your webcam.

### Step 2 — Run the MATLAB simulation
Open `matlab/quarter_car_live.m` in MATLAB and run it.
MATLAB connects to Python via TCP and the live simulation starts.

### Step 3 — Point the webcam at road images
The system detects road conditions in real time and you'll see the damping coefficient and suspension response update live in MATLAB.

---

## 📊 Results

### Pothole Detection → Adaptive Response
The adaptive system (cs = 5000 N·s/m) significantly reduces body oscillation compared to the passive system (cs = 1500 N·s/m) when a pothole is detected.

### Passive vs Adaptive — Key Differences
- **Body acceleration peaks**: ~40% lower in adaptive system
- **Settling time**: adaptive system stabilizes 2–3x faster
- **Suspension deflection**: better controlled in adaptive system

*(Add your result screenshots from the report here)*

---

## 🔮 Future Scope

- [ ] Extend to **BEV platform** — add battery pack sub-mass and regenerative braking pre-activation
- [ ] Replace bounding-box distance estimation with **LiDAR or stereo vision**
- [ ] Scale from quarter-car to **full-car model**
- [ ] Integrate with **CAN bus** for real ECU communication
- [ ] Hardware-in-loop testing on actual vehicle

---

## 👥 Team

| Name | Role |
|---|---|
| J. Rishi Kumar | Dataset collection & YOLOv8 model training |
| G. Vinay Kumar | System integration & performance analysis |
| I. Rohith Sai | MATLAB suspension modelling & simulation |
| A. Sai Dinesh | Literature review & methodology |
| D. Teja | Documentation, testing & result visualisation |

**Guide:** Dr. Kiran Jasthi, Assistant Professor, CSE (AIML & IoT), VNRVJIET

---

## 🏫 Institution

**Vallurupalli Nageswara Rao Vignana Jyothi Institute of Engineering & Technology (VNRVJIET)**
B.Tech Automobile Engineering | Minor: AI/ML | May 2026

---

## 📬 Contact

**J. Rishi Kumar**
📧 jrishikumar025@gmail.com
📱 6305778750
🔗 [LinkedIn](linkedin.com/in/rishi-kumar-vnr)

---

> *"What if your car could see the road before you feel it?"*
