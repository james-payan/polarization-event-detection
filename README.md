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

**Figures in the paper only:** The manuscript may include a full data pipeline schematic (e.g. `datapipeline.png`). That asset is **not** part of this repository; only analysis figures and EDA assets under `figures/` are provided here.

## What is excluded

- Raw Twitter/X data and API credentials.
- Full LaTeX manuscript (keep private until publication). When the paper exists, add a **DOI**, **arXiv** link, or journal reference in this README.

## Data and privacy

To respect platform and sharing rules, this bundle includes only **derived, anonymized fields** (e.g. surrogate IDs, timestamps, precomputed Likert labels)—**not** raw tweet text or real user identifiers.

**Stance labels:** The column `likert_scale_Q1` in `data/PensionReform.csv` is **already computed** (LLM-based stance coding in the study). This public bundle does **not** ship the full labeling pipeline or raw posts, to reduce risk of re-identifying individuals. **Full labeling code or scripts can be shared on reasonable request** when doing so does not conflict with platform terms or privacy obligations.

## Python version

**Python 3.10.x** (reference: **3.10.15** in `trabajo_integrador`, Windows). Recreate a virtual environment or conda env, then `pip install -r requirements.txt`.

## Dependencies (`requirements.txt`)

Versions were taken from **`conda list`** and **`pip freeze`** in **`conda activate trabajo_integrador`** (Miniconda). The **`measures`** package (providing `MECNormalized`) is installed from Git at the same commit as that environment:

`https://github.com/Ulvenforst/pol_measures.git@2dc1ae40e31aa5be98f7b4c93eec464c148f8a59`

If you use a fork or newer commit, update that line and re-run the notebook before publishing.

## Running the notebook

1. From this directory (the public repo root after you copy it), create a venv/conda env and run `pip install -r requirements.txt`.
2. Open `notebooks/event_detection_pension_reform_public.ipynb`.

**Working directory:** The notebook discovers the bundle root by searching upward for `event-detection/article-files/data/PensionReform.csv` or `data/PensionReform.csv`, so it runs whether the kernel’s current directory is the monorepo root, this folder, or `notebooks/`. If another unrelated `data/PensionReform.csv` exists at the repository root, the path under `event-detection/article-files/` is preferred.

**Headless execution:** From the **monorepo** repository root (parent of `event-detection/`):

`conda run -n trabajo_integrador python -c "import nbformat; from nbclient import NotebookClient; nb=nbformat.read(open('event-detection/article-files/notebooks/event_detection_pension_reform_public.ipynb',encoding='utf-8'),as_version=4); NotebookClient(nb,timeout=600).execute()"`

After copying to a **standalone** public repo, adjust the path to `notebooks/event_detection_pension_reform_public.ipynb` and run from that repo’s root.

On some setups, `jupyter nbconvert --execute` fails because of a **mistune / nbconvert** import mismatch; **`nbclient`** (listed in `requirements.txt`) avoids that path.

## Article tables (reproducibility checklist)

These are the manuscript targets the notebook is written to match. **Table `tab:detected_events`** reports rounded metrics; the notebook may print more digits (e.g. 0.714286 vs **0.71** precision).

| Item | Content |
|------|---------|
| **`tab:final_config`** | Sentiment baseline: **W=3**, **K=1.1**. Volume: **W=2**, **K=1.7**. MEC: **W=2**, **λ=0.2**. |
| **`tab:detected_events`** | Tweet volume: Recall **0.42**, Precision **0.71**, F1 **0.53**. MEC: **0.75** / **0.75** / **0.75**. |
| **`tab:detailed_detected_events`** | Per-day checkmarks for sentiment vs volume vs MEC for dates **2024-06-23** through **2024-07-19** (see notebook output table). |

**Verified (2026-04-02):** The notebook was executed end-to-end with **`trabajo_integrador`** on this repo using **`nbclient`** from the monorepo root; metrics printed in the evaluation section match the article values within rounding.

## Clearing outputs before git push

Prefer a clean notebook JSON for version control:

`jupyter nbconvert --clear-output --inplace notebooks/event_detection_pension_reform_public.ipynb`

(if `nbconvert` imports correctly in your env), or clear outputs from the Jupyter UI.

## License

This project is released under the **MIT License**; see the `LICENSE` file in the repository root.
