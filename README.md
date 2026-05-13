# 🌊 Ocean Eyes — Dive Spot Climate Risk Index

A data project quantifying climate-related thermal stress risk for 50 globally recognized dive spots, using 41 years of satellite data from NOAA Coral Reef Watch (1985–2025).

**→ [View the live dashboard on Ocean Eyes](https://oceaneyes.lovable.app)**

---

## What this project does

Rising ocean temperatures are one of the biggest threats to coral reefs worldwide. This project builds a **Composite Climate Risk Index** for dive spots across the Indo-Pacific, Indian Ocean, Red Sea, Caribbean, and Atlantic — combining historical bleaching frequency and long-term SST warming trends into a single, interpretable risk score (Low / Medium / High / Critical).

---

## Notebooks

| Notebook | Description |
|---|---|
| `01_data_preparation.ipynb` | Loads NOAA CRW NetCDF files, builds spot list, nearest-reef-pixel join → `panel_dataset.csv` |
| `02_risk_score.ipynb` | Normalizes metrics, calculates weighted composite score, classifies risk levels |
| `03_visualizations.ipynb` | Builds interactive Plotly charts: world map, ranking, scatter plot, spot detail templates |

---

## Data

- **Source:** NOAA Coral Reef Watch Thermal History Product Suite v3.7
- **Coverage:** 1985–2025 (41 years), 5km resolution
- **Variables used:** Bleaching stress frequency (DHW ≥ 4) and warm season SST trend (°C/decade)
- **Note:** Raw `.nc` files (~560MB) are not included in this repo. Download directly from [NOAA CRW](https://coralreefwatch.noaa.gov/product/thermal_history/)

---

## Methodology

See [`methodology.md`](./methodology.md) for a full explanation of the data processing pipeline, spatial join approach, risk score calculation, and limitations.

---

## Output files

| File | Description |
|---|---|
| `panel_dataset.csv` | Final dataset: 49 spots with bleaching frequency, SST trend, risk score and risk level |
| `world_map.html` | Interactive world map colored by risk level |
| `top20_risk_ranking.html` | Horizontal bar chart of top 20 most at-risk spots |
| `scatter.html` | Scatter plot: bleaching frequency vs. SST trend |
| `spots.csv` | Input spot list with coordinates |

---

## Tech stack

Python · pandas · numpy · xarray · scipy · plotly 

---

*Data source: NOAA Coral Reef Watch, https://coralreefwatch.noaa.gov*
