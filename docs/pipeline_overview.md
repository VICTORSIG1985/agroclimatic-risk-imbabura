# Pipeline Overview

## Five-Phase Architecture

```
Phase 1 — Data Preparation (Scripts 00–02)
    ├── 00: Project configuration and environment validation
    ├── 01: BASD-CMIP6-PE download and preprocessing (10 GCMs × 3 SSPs)
    └── 02: Spatial clipping to Imbabura bounding box + 10 km buffer

Phase 2 — Agroclimatic Index Calculation (Scripts 03A–03F)
    ├── 03A: Reference evapotranspiration ET₀ (Hargreaves-Samani, 1985)
    ├── 03B: Daily and annual water deficit (P − ET₀)
    ├── 03C: Crop-specific thermal stress days (4 thermal thresholds)
    ├── 03D: Agricultural drought indices (CDD, dry spells 7d/15d)
    ├── 03E: Frost days (Tmin < 0°C)
    └── 03F: Temporal aggregation → 16 agroclimatic indices

Phase 3 — Exposure Quantification (Scripts 04B–04C)
    ├── 04B: Multi-source data assessment (MapSPAM, ESPAC, MAG-WFS)
    └── 04C: Exact zonal statistics disaggregation (exactextract)

Phase 4 — Random Forest SDM (Scripts 05A–06C)
    ├── 05A: GBIF occurrence download and cleaning (28,965 → 2,681 records)
    ├── 05B: RF training dataset construction (presences + pseudo-absences)
    ├── 05C: Thermal variable addition
    ├── 06:  RF training + spatial cross-validation (k=5, 2° latitude blocks)
    ├── 06B: Projection to 360 scenario combinations (4 × 10 × 3 × 3)
    └── 06C: Parish-level aggregation (within + nearest neighbor)

Phase 5 — Bayesian Network Integration + Products (Scripts 07–10)
    ├── 07: Bayesian Network (7 nodes, 6 edges, 1,512 inferences)
    ├── 08: 58 cartographic products (300 DPI, UTM Zone 17S)
    ├── 09: 42 parish technical sheets (PDF, automated recommendations)
    └── 10: Final validation + ISO 19115 audit
```

## Risk Index Formula

IR = 0 × P(Low) + 0.5 × P(Medium) + 1 × P(High)

Where probabilities are obtained via Variable Elimination inference (pgmpy v0.1.25).

## CMIP6 Ensemble — 10 GCMs

CNRM-CM6-1, CNRM-ESM2-1, CanESM5, EC-Earth3, GFDL-ESM4,
IPSL-CM6A-LR, MIROC6, MPI-ESM1-2-HR, MRI-ESM2-0, UKESM1-0-LL

Strategy: equal-weight ensemble mean ("model democracy", Knutti et al., 2010)
