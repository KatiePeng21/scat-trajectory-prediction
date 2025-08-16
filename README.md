# SCAT Flight Trajectory Prediction Pipeline

This repository contains my **individual branch work** from the **Vanguard Defense HUVTSP internship** project on aircraft trajectory prediction.  
While the overall internship was a group effort, all code and experiments here are my own, originally developed under the branch `SCAT-katie` and now published as a standalone repository.

The pipeline processes, analyzes, and forecasts aircraft trajectories using the **SCAT dataset**. It uses a **GRU-based sequence-to-sequence model** for multi-step forecasting, predicting future relative displacements in meters based on recent motion history.

> **Note:** Due to file size limits, most datasets and generated visualizations are excluded via `.gitignore`. The folder structure is preserved for reproducibility.

---

## Folder Structure

- `data/validated_data/` – Cleaned datasets (Parquet format)  
- `ml_notebooks/` – Jupyter notebooks for analysis and experiments  
- `plots/` – Visualization outputs (excluded due to size)  
- `scripts/` – Data loading, preprocessing, visualization, and training scripts  
- `streamlit/` – Annotation UI for validating trajectories  
- `.gitkeep` – Placeholder files for empty directories  

---

## Dataset Overview

The SCAT dataset includes:  
- Flight plans, clearances, and surveillance data (lat, lon, alt, time)  
- Predicted trajectories from ATC systems  
- Airspace sector information  

**Source:** [Mendeley Data – SCAT](https://data.mendeley.com/datasets/8yn985bwz5/1)  
**Size:** ~5.5 GB compressed, >100,000 JSON files  

---

## Background

This work was completed during my **Vanguard Defense** internship through the **Harvard Undergraduate Ventures-TECH Summer Program (HUVTSP)**.  
The main project focused on trajectory prediction for defense applications.  
This repository represents my **independent implementation** of the SCAT pipeline.

---

## Problem Statement

**Goal:** Predict future flight positions using recent trajectory data and derived motion features.  
**Application:** Improve situational awareness in defense and air traffic control systems, enabling faster, more informed operational decisions.

---

## Differences from Group Model

- Uses **relative displacements in meters** rather than absolute latitude/longitude.  
- Incorporates **derived motion features** (ground speed components and climb rate) as inputs.  
- Employs a **GRU encoder–decoder architecture with teacher forcing** for multi-step rollout.  
- Produces **geodesic error in meters** for clear, operationally relevant evaluation.  
- Does **not** currently include weather or time-to-step predictions, which the group model may handle.

---

## Pipeline Overview

1. **Data Merge** – Combine SCAT ZIP archives into unified files.  
2. **Cleaning & Validation** – Remove invalid rows, duplicates, and short flights.  
3. **Split Generation** – Create `train_ids.txt`, `val_ids.txt`, `test_ids.txt` for reproducibility.  
4. **Single-File Storage** – Save cleaned flights in `validated_cleaned.parquet`.  
5. **Data Loader** – `load_dataset.py` for loading splits from Parquet.  
6. **Trajectory Visualization** – 2D/3D plotting with gradients and markers.  
7. **Annotation Tool** – Streamlit + Plotly for quick trajectory validation.  
8. **Feature Engineering** – Ground speed components, climb rate, normalization.  
9. **Dataset Creation** – PyTorch sequence dataset for model training/testing.  
10. **GRU Forecast Model** – Encoder–decoder architecture with teacher forcing.  
11. **Training Loop** – GPU support, mixed precision, gradient clipping.  
12. **Evaluation** – Geodesic error in meters, prediction vs actual plots.

---

## Setup Instructions

```bash
git clone https://github.com/KatiePeng21/scat-trajectory-prediction.git
cd scat-trajectory-prediction

python -m venv venv
# Activate:
# Windows:
venv\Scripts\activate
# macOS/Linux:
source venv/bin/activate

pip install -r requirements.txt
