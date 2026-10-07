# Multiobjective Optimization of Urban Rain-Gauge Networks

**Reallocating rainfall monitoring stations by integrating Geoprocessing and Operational Research**

🌐 **Language / Idioma:** **English** | [Português](README.pt-BR.md)

---

[![Python](https://img.shields.io/badge/Python-3.10%2B-blue.svg)](https://www.python.org/)
[![Jupyter](https://img.shields.io/badge/Jupyter-Notebook-orange.svg)](https://jupyter.org/)
[![Solver](https://img.shields.io/badge/Solver-PuLP%20%2F%20CBC-success.svg)]()
[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)
[![Status](https://img.shields.io/badge/status-research-blueviolet.svg)]()

> Part of the PhD research in Operational Research at **UNIFESP / ITA**.
> See the research overview: [PhD-Research-Operational-Research](https://github.com/roberval1994).

## Overview

This project proposes and compares methodologies to **reallocate and optimize rain-gauge
stations** in the city of São José dos Campos (SP, Brazil). It integrates
**geoprocessing** and **operational research** to maximize network efficiency over
critical areas, driven by a **multiobjective weight** (`W_exp`) that combines
**population density**, **hydrological risk zones**, and **precipitation volume**.

## Problem framing

Each candidate point in a spatial grid receives a composite score derived from three
layers. Optimization then selects station locations that maximize the covered weight.

| Layer | Source | Processing |
|---|---|---|
| **Rainfall** | Real 2025 data (PlugField) | Outlier removal of faulty stations |
| **Risk** | Flood / geological polygons (GeoSanja/SJC) | 800 m metric buffer for spatial uncertainty |
| **Population** | WorldPop 2025 (100 m resolution) | Cumulative population within a 1,000 m radius |

Precipitation is interpolated with **Ordinary Kriging** to produce a continuous rainfall
surface feeding the optimization model.

## Optimization approaches compared

1. **Weighted K-Means (baseline)** — regionalizes the territory by weight density and places stations at cluster centroids.
2. **Greedy heuristic with spatial inhibition** — iteratively picks the highest-weight points, enforcing a minimum distance (1.5 km) to avoid redundancy.
3. **Exact Maximal Covering Location Problem (MCLP)** — Integer Linear Programming (PuLP / CBC) finding the globally optimal coverage within a fixed service radius (800 m).
4. **Hybrid algorithms (spatial decomposition)**
   - *Hybrid 1*: K-Means clustering + greedy heuristic per region.
   - *Hybrid 2*: K-Means clustering + exact MCLP per sub-region (precision of the exact model with the speed of decomposition).

## Scenarios & metrics

Scenarios test different priority balances — **Balanced**, **Risk-focused**,
**Population-focused**. Performance is evaluated by:

- Population coverage gain (**Pop %**)
- Risk coverage gain (**Risk %**)
- Total score
- Runtime (s)

Interactive **Folium** maps render risk heatmaps and station coverage radii.

## Repository structure

```
.
├── notebooks/
│   ├── Projeto_Artigo.ipynb                  # Main modelling & optimization pipeline
│   └── Artigo_Otimizacao_Pluviometros.ipynb  # Experiments and comparisons
├── data/                                     # Processed grids (.parquet), weights (.csv)
├── maps/                                     # Interactive Folium maps (.html)
├── results/                                  # Metric tables and comparison outputs
├── requirements.txt
├── LICENSE
├── README.md                                 # English (this file)
└── README.pt-BR.md                           # Portuguese
```

## Data

> **Heavy datasets** (e.g. the ~457 MB WorldPop population raster) are **not versioned**
> in this repository. See [`Dados/LEIA-ME-dados.md`](Dados/LEIA-ME-dados.md) for download instructions,
> sources (WorldPop, GeoSanja/SJC, PlugField), and the expected folder layout.

## Getting started

```bash
git clone https://github.com/roberval1994/Multiobjective-Optimization-of-Urban-Rain-Gauge-Networks.git
cd Multiobjective-Optimization-of-Urban-Rain-Gauge-Networks

python -m venv .venv
.venv\Scripts\activate          # Windows
# source .venv/bin/activate     # Linux / macOS
pip install -r requirements.txt

jupyter notebook
```

## Key results

- Four optimization strategies benchmarked under three priority scenarios.
- Exact MCLP provides the optimal coverage baseline; hybrids recover most of the gain at a fraction of the runtime.
- Interactive maps make coverage and risk trade-offs auditable.

## Tech stack

`Python` · `GeoPandas` · `Shapely` · `PyKrige` · `PuLP` (CBC) · `scikit-learn` (K-Means) · `Folium` · `pandas` · `NumPy`

## Author

**Roberval Gonçalves Moreira Filho**
Data Scientist | Operational Research Analyst — PhD candidate, UNIFESP/ITA

[![Email](https://img.shields.io/badge/Email-roberval.researcher.or%40outlook.com-red)](mailto:roberval.researcher.or@outlook.com)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-robervalOr-blue)](https://www.linkedin.com/in/robervalOr)
[![GitHub](https://img.shields.io/badge/GitHub-roberval1994-black)](https://github.com/roberval1994)

## License

Released under the MIT License. See [LICENSE](LICENSE).
