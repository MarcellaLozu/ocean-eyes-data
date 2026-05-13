# Ocean Eyes — Climate Risk Index: Methodology

## Overview

The Ocean Eyes Climate Risk Index quantifies the thermal stress risk for 50 globally recognized dive spots using publicly available satellite data from NOAA Coral Reef Watch (CRW). The index is designed to be transparent, reproducible, and accessible to non-scientific audiences.

---

## Data Source

**NOAA Coral Reef Watch Thermal History Product Suite v3.7**
- Provider: NOAA / NESDIS / STAR Coral Reef Watch Program
- Temporal coverage: 1985–2025 (41 years)
- Spatial resolution: 5km (1/20°)
- Access: https://coralreefwatch.noaa.gov/product/thermal_history/

Two datasets were used:

| Dataset | Variable | Description |
|---|---|---|
| Stress Frequency | `n_ge4` | Number of years with significant bleaching-level heat stress (DHW ≥ 4) |
| Stress Frequency | `n_ge8` | Number of years with severe bleaching-level heat stress (DHW ≥ 8) |
| SST Trend | `trend_warmseason` | Sea Surface Temperature warming trend (°C per decade) |

Degree Heating Weeks (DHW) is a cumulative measure of thermal stress above the local maximum monthly mean temperature. DHW ≥ 4 is associated with significant coral bleaching; DHW ≥ 8 with widespread bleaching and mortality.

---

## Spot Selection

50 globally recognized dive spots were selected to represent coral reef ecosystems across all major ocean regions: Indo-Pacific, Indian Ocean, Red Sea, Caribbean, and Atlantic/Pacific outliers. Spots were defined by their central coordinates (latitude/longitude).

---

## Spatial Join (Nearest Reef Pixel)

NOAA CRW data is provided as a global raster grid. For each dive spot, the nearest grid pixel with `reef_mask = 1` was identified using a KD-Tree nearest-neighbor search (scipy.spatial.cKDTree), restricted to reef-classified pixels only. This ensures that coastal spots are not incorrectly mapped to adjacent open-ocean pixels.

Of 50 spots, 49 returned valid data. One spot (Red Sea – Blue Hole Dahab) returned no reef-classified pixel within the search radius and is excluded from the risk scoring.

---

## Risk Score Calculation

The Composite Climate Risk Score is calculated in three steps:

### Step 1 — Min-Max Normalization

Both input metrics are normalized independently to a 0–100 scale:

```
normalized = (value - min) / (max - min) × 100
```

This ensures that differences in units and magnitude between bleaching frequency (count) and SST trend (°C/decade) do not distort the composite score.

### Step 2 — Weighted Composite Score

```
Risk Score = 0.6 × score_bleaching + 0.4 × score_trend
```

**Bleaching Frequency (60%)** is weighted higher as it reflects directly observed coral stress events over 41 years — a more immediate indicator of ecological damage.

**SST Trend (40%)** reflects the rate of long-term ocean warming — a forward-looking indicator of future risk.

### Step 3 — Risk Level Classification

| Risk Score | Risk Level |
|---|---|
| 0 – 24 | 🟢 Low |
| 25 – 49 | 🟡 Medium |
| 50 – 74 | 🟠 High |
| 75 – 100 | 🔴 Critical |

---

## Limitations

- The index reflects **historical thermal stress** (1985–2025) and long-term SST trends. It does not represent current real-time conditions.
- Spatial resolution is 5km — localized reef conditions (currents, depth, shading) are not captured.
- The weighting (60/40) is an analytical choice based on ecological reasoning, not empirically derived from field validation data.
- Spots outside the NOAA reef mask (e.g. Azores, Galápagos) may reflect different ecological dynamics than tropical coral reefs and should be interpreted with caution.

---

## Reproducibility

All data processing and scoring is performed in Python using open-source libraries (pandas, numpy, xarray, scipy). The full codebase and raw output data are available in the project repository.

**Data citation:**
NOAA Coral Reef Watch. 2026. NOAA Coral Reef Watch Thermal History Version 3.7. College Park, Maryland, USA: NOAA Coral Reef Watch. https://coralreefwatch.noaa.gov/product/thermal_history/

---

*Last updated: May 2026 | Ocean Eyes — https://oceaneyes.lovable.app/
