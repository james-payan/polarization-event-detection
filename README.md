[![DOI](https://zenodo.org/badge/1164342838.svg)](https://doi.org/10.5281/zenodo.19464364)
# Polarization-based event detection on X (Twitter) data

This repository contains code and notebooks used to study **event detection in social media** using a **polarization-based signal** (MEC—minimum effort consensus) alongside baseline approaches based on **posting volume** and **stance-derived sentiment**.

The case study is public debate around the **Colombian pension reform (June–July 2024)** using posts collected from **X (formerly Twitter)**.

## Project goal

The main objective is to evaluate whether **polarization dynamics** can serve as a reliable signal for detecting socially relevant events in social media conversations, compared to volume- and sentiment-based detectors.

## What’s included

- Data loading and daily time-series construction from anonymized Likert stance labels (**precomputed in `PensionReform.csv`**; see note below)
- Baseline event detection (tweet volume and sentiment proportions) with parameter search; the sentiment baseline applies a **local significance filter** on candidate spike days (implementation in the notebook—the manuscript omits this detail to keep the main text readable)
- Polarization-based event detection using **MEC** ($\Delta$MEC vs IQR thresholds)
- Parameter exploration, final configurations, and evaluation (precision, recall, F1) against the sentiment baseline as reference
- Supporting figures (e.g. MEC overview) and reproducibility metadata (`requirements.txt`)

## Repository structure

| Path | Purpose |
|------|---------|
| `data/PensionReform.csv` | Anonymized posts (Likert stance codes only; no raw tweet text). |
| `notebooks/event_detection_pension_reform_public.ipynb` | Main reproducible analysis aligned with the paper’s tables and figures. |
| `figures/` | Static assets for EDA (`termometers.png`, `escalas_polarizacion.png`); notebook can write `modelo_polarizacion_final.png` here. |
| `requirements.txt` | Pinned dependencies from conda env `trabajo_integrador` (see below). |


## What is excluded

- Raw Twitter/X data and API credentials.

## Data and privacy

To respect platform and sharing rules, this bundle includes only **derived, anonymized fields** (e.g. surrogate IDs, timestamps, precomputed Likert labels)—**not** raw tweet text or real user identifiers.

**Stance labels:** The column `likert_scale_Q1` in `data/PensionReform.csv` is **already computed** (LLM-based stance coding in the study). This public bundle does **not** ship the full labeling pipeline or raw posts, to reduce risk of re-identifying individuals. **Full labeling code or scripts can be shared on reasonable request** when doing so does not conflict with platform terms or privacy obligations.

## Python version

**Python 3.10.x** . Recreate a virtual environment, then `pip install -r requirements.txt`.


## Running the notebook

1. From this directory (the public repo root after you copy it), create a venv/conda env and run `pip install -r requirements.txt`.
2. Open `notebooks/event_detection_pension_reform_public.ipynb`.

**Working directory:** The notebook discovers the bundle root by searching upward for `data/PensionReform.csv`.


**Notebook outputs in this repository:** The checked-in copy of `notebooks/event_detection_pension_reform_public.ipynb` **includes executed cell outputs** (tables and figures) on purpose. Readers can review results on GitHub or in Jupyter **without** installing Python or running the environment. To reproduce or update numbers locally, run all cells after `pip install -r requirements.txt`.

## License

This project is released under the **MIT License**; see the `LICENSE` file in the repository root.
