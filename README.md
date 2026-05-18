# Percepta — Public Safety AI System

> Real-time weapon detection and crowd behaviour anomaly detection for public spaces.

Percepta is an end-to-end AI perception system designed for CCTV-style environments such as airports, shopping centres, and transit hubs. It detects threats, tracks individuals across frames, evaluates risk in real time, and logs every event with a plain-language explanation.

---

## Demo

> 📹 *Demo video coming soon*
> 
> 🌐 *Live deployment URL coming soon*

---

## What it does

Percepta runs a continuous pipeline on video input:

1. **Detects** weapons (knives, handguns) and persons using a fine-tuned YOLOv8 model
2. **Tracks** individuals across frames with persistent IDs via DeepSORT
3. **Analyses** crowd behaviour — sudden dispersal, running, panic clustering
4. **Evaluates risk** using a three-tier rule engine (LOW / MEDIUM / HIGH)
5. **Generates explanations** — plain-language descriptions of every alert
6. **Logs everything** to a PostgreSQL database via a FastAPI backend
7. **Displays** live video, bounding boxes, risk level, and alert feed on a Streamlit dashboard

The key differentiator is the **dual-signal approach**: a weapon detection alone triggers a MEDIUM alert, but weapon detection combined with crowd anomaly escalates to a CRITICAL alert — mirroring how real security systems operate.

---

## System architecture

```
Video input (CCTV / file)
    ↓
Object detection (YOLOv8)
    ↓
Multi-object tracking (DeepSORT)
    ↓
Crowd anomaly analysis
    ↓
Risk engine (rule-based, 3 tiers)
    ↓
Explanation generation (template-based)
    ↓
FastAPI backend → PostgreSQL event log
    ↓
Streamlit dashboard
```

---

## Risk tiers

| Tier | Trigger | Example explanation |
|------|---------|---------------------|
| 🟢 LOW | Person detected | "Person tracked near entrance — no threat indicators" |
| 🟡 MEDIUM | Weapon detected | "Knife detected near escalator — high confidence" |
| 🔴 HIGH | Weapon + crowd anomaly | "Knife detected — crowd dispersal event in progress — critical alert" |

---

## Tech stack

| Component | Technology |
|-----------|-----------|
| Object detection | YOLOv8 (Ultralytics) |
| Multi-object tracking | DeepSORT |
| Video processing | OpenCV |
| Backend API | FastAPI |
| Database | PostgreSQL |
| Dashboard | Streamlit |
| Containerisation | Docker |
| Deployment | AWS / GCP |
| Language | Python 3.10+ |

---

## Datasets

The detection model was trained on a merged dataset from three sources:

| Dataset | Source | Images | Classes |
|---------|--------|--------|---------|
| Simuletic CCTV Knife Detection | RoboFlow | ~1,000 | person, knife |
| yolov7test Weapon Detection | RoboFlow | ~9,700 | knife, handgun, pistol, rifle, person |
| Violence & Weapon Detection | RoboFlow | ~4,150 | person, weapon |

All datasets are pre-existing — no custom data was collected. Datasets were merged, class-balanced, and exported in YOLOv8 format prior to training.

---

## Model performance

| Metric | Value |
|--------|-------|
| mAP@0.5 | *TBD after training* |
| Precision | *TBD* |
| Recall | *TBD* |
| False positive rate | *TBD* |

> Results will be updated after Week 2–3 training runs.

---

## Project structure

```
percepta/
├── src/
│   ├── detection/        # YOLOv8 inference wrapper
│   ├── tracking/         # DeepSORT integration
│   ├── anomaly/          # Crowd behaviour analysis
│   ├── risk_engine/      # Rule-based risk evaluator
│   └── explainer/        # Template-based explanation generator
├── dashboard/            # Streamlit app
├── api/                  # FastAPI backend
├── models/               # Trained model checkpoints
├── data/                 # Dataset configs (not raw data)
├── logs/                 # Event log samples
├── docs/                 # Architecture diagrams, notes
├── docker/               # Dockerfile + compose
├── tests/
├── requirements.txt
└── README.md
```

---

## Getting started

### Prerequisites

- Python 3.10+
- Docker
- PostgreSQL (or use the Docker Compose setup)
- GPU recommended for inference (CUDA 11.8+)

### Installation

```bash
git clone https://github.com/yourusername/percepta.git
cd percepta
pip install -r requirements.txt
```

### Run with Docker

```bash
docker-compose up --build
```

### Run locally

```bash
# Start the API
uvicorn api.main:app --reload

# Start the dashboard
streamlit run dashboard/app.py
```

### Run inference on a video file

```bash
python src/detection/run.py --source path/to/video.mp4
```

---

## Event log schema

Every detected event is logged to PostgreSQL with the following fields:

```
timestamp     | TIMESTAMPTZ
object_class  | TEXT         -- knife, handgun, person
confidence    | FLOAT
risk_level    | TEXT         -- LOW, MEDIUM, HIGH
track_id      | INT          -- persistent DeepSORT ID
explanation   | TEXT         -- plain-language description
frame_number  | INT
```

---

## Roadmap

- [x] Project scoping and dataset selection
- [ ] Environment setup and dataset merging
- [ ] YOLOv8 training and evaluation
- [ ] DeepSORT tracking integration
- [ ] Crowd anomaly detection
- [ ] Risk engine and explanation generator
- [ ] FastAPI backend + PostgreSQL logging
- [ ] Streamlit dashboard
- [ ] Docker containerisation
- [ ] Cloud deployment
- [ ] Demo video

**Post-MVP (if time permits)**
- Abandoned object detection
- Restricted zone intrusion alerts
- LLM-powered explanation generation (v2)

---

## About

Built as a portfolio project by a final-year AI student targeting applied AI and computer vision engineering roles. Percepta demonstrates end-to-end AI system design — from model training through to cloud deployment — applied to a real-world public safety problem.

**Portfolio:** *coming soon*  
**LinkedIn:** *coming soon*
