# FraudSentinel: Bank-Side Real-Time Fraud Prevention & Geospatial Intelligence System

[![Python 3.11](https://img.shields.io/badge/Python-3.11-blue.svg)](https://www.python.org/)
[![FastAPI](https://img.shields.io/badge/FastAPI-0.115+-009688.svg)](https://fastapi.tiangolo.com/)
[![React 18](https://img.shields.io/badge/React-18.3-61DAFB.svg)](https://react.dev/)
[![TypeScript](https://img.shields.io/badge/TypeScript-5.6-3178C6.svg)](https://www.typescriptlang.org/)
[![LightGBM](https://img.shields.io/badge/LightGBM-Champion_v1-brightgreen.svg)](https://lightgbm.readthedocs.io/)
[![TreeSHAP](https://img.shields.io/badge/Explainable_AI-TreeSHAP-orange.svg)](https://shap.readthedocs.io/)
[![GIS Mapping](https://img.shields.io/badge/Leaflet-OpenStreetMap-green.svg)](https://leafletjs.com/)

**Academic Prototype for Final-Year B.Tech (Artificial Intelligence & Data Science)**  
*Domain: Core Banking Payment Switches, Real-Time Transaction Ingestion & Financial Risk Operations*

> **Disclaimer:** FraudSentinel is an institutional bank-side prevention system operating on synthetic transaction streams and certified hardware registries. It never connects to real payment networks, never alters live cardholder funds, and complies with financial safety standards.

---

## 🏛️ System Architecture

FraudSentinel intercepts transaction events directly at the **bank payment switch** as cards are swiped at merchant POS terminals, withdrawn from ATMs, or transacted on e-commerce payment rails.

```
       [ Physical POS Terminals / ATM Networks / Online Payment Gateways ]
                                      │
                                      ▼ (ISO 8583 / JSON Switch Event)
                        [ Bank Payment Switch Engine ]
                                      │
                                      ▼
┌──────────────────────────────────────────────────────────────────────────────┐
│                  FraudSentinel Real-Time Intelligence Pipeline               │
│                                                                              │
│  1. Terminal & IP Registry Lookup (Certified POS/ATM Hardware & IP Subnet)   │
│  2. Customer Baseline Retrieval (32+ Deep Historical Norms & Spending Bands) │
│  3. Geospatial Feature Engineering (Haversine Distance & Travel Velocity)    │
│  4. 13-Feature Behavioral Extraction (Exact Match to Trained Model Contract) │
│  5. Dual ML Scoring (LightGBM Supervised Champion + Isolation Forest Novelty)│
│  6. Platt Calibration & 4-Tier Policy (Approve · Verify · Review · Decline)  │
│  7. TreeSHAP Feature Attribution (Risk Elevating vs Protective Drivers)      │
│  8. SSE Live Event Publication (Standardized {event_type, data} Envelope)    │
└──────────────────────────────────────┬───────────────────────────────────────┘
                                       │
                                       ▼ (Real-Time SSE Stream)
               [ Bank Analyst Operations Center & GIS Radar ]
     (OpenStreetMap Radar · Investigation Queue · Dossiers · Simulated Card Lock)
```

---

## 🌟 Key Capabilities

1. **Certified Hardware Terminal Registry**: Physical locations are resolved from registered POS/ATM hardware (`POS-CJB-001`, `POS-DEL-001`, `ATM-BOM-001`) or IP subnet geolocation. Cardholders never manually select GPS coordinates.
2. **Geospatial Transit & Velocity Vectoring**: Computes great-circle distance via the spherical **Haversine formula** and calculates physical travel speed ($\text{km/h}$). Instantly detects impossible travel velocities ($\gg 800\text{ km/h}$) while approving legitimate paced travel ($\approx 60\text{ km/h}$).
3. **Persistent Multi-Week Customer Baselines**: Synthetic cardholders (e.g. `DEMO001` Aarav Patel with 32 historical transactions across 28 days) provide deep baselines for amount $z$-scores, spending ratios, and habitual hours.
4. **Ensemble Machine Learning**: 
   - **LightGBM Champion v1**: Supervised gradient-boosted decision trees (ROC-AUC **0.9889**, p95 latency **1.8 ms**).
   - **Isolation Forest**: Unsupervised novelty detector protecting cold-start cardholders (`CUST00005`).
   - **Platt Sigmoid Calibration**: Empirical probability calibration across 4 policy tiers (`approve`, `verify`, `review`, `decline`).
5. **Bidirectional TreeSHAP Explainability**: Decomposes individual predictions into risk-increasing drivers versus protective baseline factors with plain-English counterfactual advice.
6. **Real-Time Leaflet OpenStreetMap GIS Radar**: Real street-level mapping covering all Indian payment corridors with zero external API keys or watermarks.
7. **Human-in-the-Loop Analyst Operations**: Complete investigation dossier workflow with simulated card freeze/unfreeze, step-up 2FA challenges, case escalation, and automated feedback loop dataset exports.

---
##
To start both the **FastAPI Backend (Port 8000)** and the **React + Vite Frontend (Port 5173)** simultaneously:

Double-click: **`run_all.bat`**

It automatically opens your browser tabs to:
* **Analyst Live Radar Map**: [http://localhost:5173/analyst/live](http://localhost:5173/analyst/live)
* **Bank Switch Simulator**: [http://localhost:5173/simulator](http://localhost:5173/simulator)
* **Analyst Credentials**: Username: `analyst` · Password: `analyst123`

---

##  Manual Terminal Launch

### 1. Launch FastAPI Backend (Port 8000)
```powershell
cd backend
..\.venv\Scripts\activate
python -m uvicorn app.main:app --host 0.0.0.0 --port 8000 --reload
```
*API interactive documentation active at [http://localhost:8000/docs](http://localhost:8000/docs).*

### 2. Launch React + Vite Frontend (Port 5173)
```powershell
cd fraudsentinel-frontend
npm run dev
```
*Frontend application active at [http://localhost:5173](http://localhost:5173).*

---

## 🧪 Automated Test Verification

Execute the complete 7-scenario bank testing suite:
```powershell
$env:PYTHONPATH="backend"
backend\.venv\Scripts\python -m pytest backend/tests/test_bank_scenarios.py -v
```

### Test Results
```
tests/test_bank_scenarios.py::test_scenario_normal_usual_location PASSED         [ 14%]
tests/test_bank_scenarios.py::test_scenario_high_value_unusual_location PASSED  [ 28%]
tests/test_bank_scenarios.py::test_scenario_stolen_card_another_city PASSED     [ 42%]
tests/test_bank_scenarios.py::test_scenario_impossible_travel PASSED            [ 57%]
tests/test_bank_scenarios.py::test_scenario_new_device_unusual_location PASSED  [ 71%]
tests/test_bank_scenarios.py::test_scenario_high_velocity_burst PASSED          [ 85%]
tests/test_bank_scenarios.py::test_scenario_normal_new_location PASSED          [100%]

======================== 7 passed in 19.06s ========================
```

---

## Project Directory Structure

```
fraud-system/
├── backend/                     # FastAPI Asynchronous REST Application
│   ├── app/
│   │   ├── main.py              # Application lifecycle, CORS, and router registration
│   │   ├── api/                 # Endpoints (Simulator, Investigations, Cards, Stream, Auth)
│   │   ├── db/
│   │   │   └── memory_store.py  # Thread-safe in-memory state engine with deep customer history
│   │   ├── ml/                  # LightGBM artifacts, 13-feature extractor, TreeSHAP explainer
│   │   ├── schemas/             # Pydantic v2 schemas for request validation & response formatting
│   │   └── services/            # Scoring pipeline, Terminal Registry, Bank Switch Simulator
│   ├── tests/
│   │   └── test_bank_scenarios.py # 7 Controlled demonstration scenarios test suite
│   └── requirements.txt         # Production backend dependencies
├── fraudsentinel-frontend/      # React 18 + Vite + TypeScript Application
│   ├── src/
│   │   ├── BankEventSimulator.tsx # Bank payment switch controller & scenario injector
│   │   ├── TransactionMap.tsx   # Leaflet OpenStreetMap GIS radar component
│   │   ├── Analyst.tsx          # Analyst Operations Center (Live, Queue, Dossiers, Analytics)
│   │   ├── Landing.tsx          # Executive institutional landing page with 3D Canvas
│   │   ├── api.ts               # Central API client, interfaces, and SSE useStream hook
│   │   └── main.tsx             # React Router routing declarations
│   └── package.json             # Frontend dependencies (React, Leaflet, Recharts, Three.js)
├── datasets_excel/              # Master reference datasets formatted for academic presentation
├── docs/                        # Complete technical documentation suite
│   ├── PROJECT_ARCHITECTURE.md  # Comprehensive system architecture & mathematical derivations
│   ├── BACKEND_SYSTEM_GUIDE.md  # Backend REST endpoints, schemas, and memory engine
│   ├── FRONTEND_SYSTEM_GUIDE.md # Frontend routes, GIS map, and analyst operations
│   ├── Final_Academic_Project_Report.md # Full academic research paper and report
│   ├── features.md              # 13 Behavioral feature definitions & equations
│   ├── model_card.md            # LightGBM & Isolation Forest governance documentation
│   └── Render_Deployment_Guide.md # Cloud production deployment instructions
├── models/                      # Trained model binaries and feature contracts
│   ├── model.joblib             # LightGBM Champion binary
│   └── features.json            # Strict 13-feature contract
├── run_all.bat                  # 1-Click launcher for both backend and frontend
├── start_backend.bat            # Standalone launcher for FastAPI backend
├── start_frontend.bat           # Standalone launcher for React frontend
└── render.yaml                  # Production blueprint configuration for Render
```

---

## Documentation Suite

* [System Architecture Specification](docs/PROJECT_ARCHITECTURE.md)
* [Backend System Guide](docs/BACKEND_SYSTEM_GUIDE.md)
* [Frontend & GIS System Guide](docs/FRONTEND_SYSTEM_GUIDE.md)
* [Final Academic Project Report](docs/Final_Academic_Project_Report.md)
* [Behavioral Feature Engineering](docs/features.md)
* [Model Governance Card](docs/model_card.md)
* [Render Cloud Deployment Guide](docs/Render_Deployment_Guide.md)
