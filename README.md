<!-- Place logo files at docs/assets/nhs-logo.png and docs/assets/reach-logo.svg -->
<p align="center">
  <img src="docs/assets/nhs-logo.png" alt="NHS Logo" height="64" />
  &nbsp;&nbsp;&nbsp;
  <img src="docs/assets/reach-logo.svg" alt="REACH Map Logo" height="64" />
</p>

![Python 3.11](https://img.shields.io/badge/python-3.11-blue.svg)
![GeoPandas](https://img.shields.io/badge/GeoPandas-0.14-green)

# REACH Map (SW): Response & Emergency Access Coverage

A Streamlit tool for NHS planners to visualise emergency response coverage, test what-if scenarios, and balance demand vs. capacity across the NHS South West.

---

## Table of Contents
- [Introduction](#introduction)
- [Methodology](#methodology)
  - [Data Sources (inputs)](#data-sources-inputs)
  - [Coverage & Metrics](#coverage--metrics)
  - [Demand & Equity Overlays](#demand--equity-overlays)
  - [Capacity & Availability (phase-2)](#capacity--availability-phase-2)
  - [Location–Allocation (optional optimiser)](#locationallocation-optional-optimiser)
- [Key Results](#key-results)
- [Relevance and Impact](#relevance-and-impact)
- [Technical Stack](#technical-stack)
- [Install & Quickstart](#install--quickstart)
- [Configuration & Data](#configuration--data)
- [Repository Structure](#repository-structure)
- [Roadmap](#roadmap)
- [License & Acknowledgements](#license--acknowledgements)

---

## Introduction
**REACH Map (SW)** is a planning tool that answers a simple question:

> *“What share of the population can we reach within **X** minutes, given our ambulance stations and acute hospitals?”*

It renders LSOA-level coverage maps, overlays age-based demand, and lets users switch sites on/off, adjust ambulances/crews and ED capacity, and compare scenarios side-by-side. The goal is strategic insight, **not** live dispatch.

---

## Methodology
Methods are deliberately simple and explainable for planning reviews. The app couples **pre-computed travel times** with transparent models.

### Data Sources (inputs)
- **Geospatial spine:** ONS 2021 LSOA boundaries & centroids (England & Wales).
- **Sites:** NHS South West acute hospitals and ambulance stations (enriched CSVs).
- **Travel times:** LSOA ↔ Site matrix (seconds) or on-the-fly isochrones for visual rings.
- **Demand (optional):** Age-weighted emergency propensity by LSOA.

### Coverage & Metrics
- **Coverage test:** An LSOA is “covered” by at least one selected site if *travel time ≤ threshold*.
- **Targets:** Presets for Cat-1 (7 min) and Cat-2 (18 min); custom thresholds allowed.
- **KPIs:**
  - % population covered within X minutes  
  - LSOAs uncovered / partially covered  
  - Coverage change vs. baseline (absolute and %)
- **Isochrones:** Optional visual bands (e.g., 7/15/30 min) around sites for rapid comprehension.

### Demand & Equity Overlays
- **Heatmap:** Age-based emergency demand per LSOA.  
- **Gap view:** High-demand LSOAs outside target time bands flagged as priority areas.

### Capacity & Availability (phase-2)
- **Ambulance availability:** Discount static coverage by a simple busy-fraction proxy from M/M/c inputs *(λ = arrivals, μ = clear time, c = crews)*.  
- **ED delay proxy:** A lightweight M/M/s panel converts ED capacity sliders into an indicative boarding/triage wait band.  
Both panels are clearly labelled as planning proxies, **not** operational predictions.

### Location–Allocation (optional optimiser)
- **MCLP (max coverage):** Given a budget *p* (sites/crews) and a time standard *S*, select the set that maximises covered demand (via `PySAL`/`spopt`).  
- **Outputs:** Recommended placements, expected coverage %, and marginal gains vs. current configuration.

---

## Key Results
This is an application, so results are scenario-dependent. The app computes and displays:
- **Coverage KPIs:** % population within 7/15/18/30 min; uncovered population; uncovered LSOAs.  
- **Scenario deltas:** Gains/losses in coverage vs. baseline after toggling sites or changing capacity.  
- **Priority gaps:** Ranked list of high-demand LSOAs failing the chosen time standard.  
- **(Optional) Optimiser picks:** Shortlist of sites/crew allocations that maximise coverage under a budget.

*Screenshots and sample CSV outputs will live in `docs/` (placeholders until your data are added).*

---

## Relevance and Impact
- **Planning-first:** Supports ICB/Trust decisions on station siting, standby points, and hospital coverage footprints.  
- **Equity-aware:** Surfaces underserved LSOAs (high demand + low coverage) to prioritise investment.  
- **Transparent:** Threshold coverage + simple queues + optional MCLP are explainable and auditable.  
- **Configurable:** Swap in new matrices, add sites, or adjust standards without code changes.

---

## Technical Stack
- **Frontend:** `streamlit`  
- **Geo & data:** `geopandas`, `pandas`, `shapely`, `pyproj`  
- **Optimisation (optional):** `pysal`, `spopt`  
- **Routing (optional):** `openrouteservice` (isochrones/matrix) or pre-computed OSRM tables  
- **Visuals:** `pydeck` or `folium` (via `streamlit-folium`)

---

## Install & Quickstart
```bash
# 1) Create environment
python -m venv .venv
# Windows: .venv\Scripts\activate
# macOS/Linux:
source .venv/bin/activate

# 2) Upgrade pip and install deps
python -m pip install --upgrade pip
pip install -r requirements.txt

# 3) Prepare config (see next section), then run
streamlit run app.py
