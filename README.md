# Agroclimatic Risk of Andean Crops under CMIP6 Scenarios in Imbabura, Ecuador

[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.19022773.svg)](https://doi.org/10.5281/zenodo.19022773)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Python 3.11](https://img.shields.io/badge/python-3.11-blue.svg)](https://www.python.org/downloads/release/python-3110/)

## Overview

This repository contains the complete reproducible pipeline for the assessment of parish-level agroclimatic risk of four Andean crops — potato (*Solanum tuberosum* L.), maize (*Zea mays* L.), common bean (*Phaseolus vulgaris* L.), and quinoa (*Chenopodium quinoa* Willd.) — under CMIP6 climate scenarios (SSP1-2.6, SSP3-7.0, SSP5-8.5) across the 42 parishes of Imbabura Province, Ecuador.

The pipeline integrates:
- **Species Distribution Models (SDM)** using Random Forest
- **Bayesian Networks** for probabilistic risk integration following the IPCC AR6 framework (Hazard × Exposure × Vulnerability)
- **BASD-CMIP6-PE** bias-corrected high-resolution climate projections (Fernandez-Palomino et al., 2024)

**Key outputs:**
- 1,512 parish-level Risk Index (IR) values
- 58 cartographic products (maps and panels)
- 42 parish technical sheets with adaptation recommendations

---

## Repository Structure

```
├── config/
│   └── config.json                  # Central configuration file (paths, crops, parameters)
├── scripts/
│   ├── SCRIPT_00_CONFIGURACION.py   # Project setup and environment validation
│   ├── SCRIPT_01_DESCARGA_CMIP6.py  # BASD-CMIP6-PE download and preprocessing
│   ├── SCRIPT_02_RECORTE_ESPACIAL.py# Spatial clipping to Imbabura bounding box
│   ├── SCRIPT_03A_ET0.py            # Reference evapotranspiration (Hargreaves-Samani)
│   ├── SCRIPT_03B_DEFICIT_HIDRICO.py# Daily water deficit
│   ├── SCRIPT_03C_ESTRES_TERMICO.py # Thermal stress days per crop
│   ├── SCRIPT_03D_SEQUIA.py         # Agricultural drought indices (CDD, dry spells)
│   ├── SCRIPT_03E_HELADAS.py        # Frost days
│   ├── SCRIPT_03F_AGREGACION.py     # Temporal aggregation of agroclimatic indices
│   ├── SCRIPT_04B_EXPOSICION.py     # Multi-source agricultural exposure data
│   ├── SCRIPT_04C_DESAGREGACION.py  # Spatial disaggregation to parish level
│   ├── SCRIPT_05A_GBIF.py           # GBIF occurrence download and cleaning
│   ├── SCRIPT_05B_DATASET_RF.py     # RF training dataset construction
│   ├── SCRIPT_05C_VARS_TERMICAS.py  # Thermal variables for RF datasets
│   ├── SCRIPT_06_RANDOM_FOREST.py   # RF training and SHAP analysis
│   ├── SCRIPT_06B_PROYECCION_RF.py  # RF projection to CMIP6 scenarios
│   ├── SCRIPT_06C_AGREGACION_PARR.py# Parish-level aggregation (within + nearest neighbor)
│   ├── SCRIPT_07_RED_BAYESIANA.py   # Bayesian Network risk integration
│   ├── SCRIPT_08_MAPAS.py           # Cartographic product generation
│   ├── SCRIPT_09_FICHAS.py          # Parish technical sheet generation
│   └── SCRIPT_10_VALIDACION.py      # Final validation and ISO 19115 audit
├── data/
│   └── sample/                      # Sample data for testing (full data via DOI links)
├── docs/
│   ├── methodology/                 # 21 methodology documentation files
│   └── figures/                     # Pipeline diagrams and conceptual figures
├── outputs/
│   ├── maps/                        # Generated risk maps (PNG/PDF)
│   └── fichas/                      # Parish technical sheets (PDF)
├── notebooks/
│   └── exploratory_analysis.ipynb   # Exploratory data analysis notebook
├── requirements.txt                 # Python dependencies
├── environment.yml                  # Conda environment specification
├── .zenodo.json                     # Zenodo metadata for DOI generation
└── LICENSE                          # MIT License
```

---

## Installation

### Option 1: Conda (recommended)
```bash
git clone https://github.com/YOUR_USERNAME/agroclimatic-risk-imbabura.git
cd agroclimatic-risk-imbabura
conda env create -f environment.yml
conda activate agroclimatic-imbabura
```

### Option 2: pip
```bash
git clone https://github.com/YOUR_USERNAME/agroclimatic-risk-imbabura.git
cd agroclimatic-risk-imbabura
pip install -r requirements.txt
```

---

## Data Requirements

The pipeline requires the following external datasets, which must be downloaded separately due to size constraints:

| Dataset | Source | Version | DOI / URL |
|---------|--------|---------|-----------|
| BASD-CMIP6-PE | Fernandez-Palomino et al. (2024) | v1.0 | [10.1038/s41597-023-02863-z](https://doi.org/10.1038/s41597-023-02863-z) |
| MapSPAM | IFPRI (2024) | v2r0 2020 | [10.7910/DVN/SWPENT](https://doi.org/10.7910/DVN/SWPENT) |
| GADM / CONALI | CONALI/INEC Ecuador | 2022 | Available at CONALI Ecuador |
| GBIF occurrences | GBIF | Feb 2026 | Downloaded via Script 05A (API) |

Once downloaded, update the paths in `config/config.json` accordingly.

---

## Pipeline Execution

Run the scripts sequentially from the `scripts/` directory:

```bash
python SCRIPT_00_CONFIGURACION.py
python SCRIPT_01_DESCARGA_CMIP6.py
python SCRIPT_02_RECORTE_ESPACIAL.py
python SCRIPT_03A_ET0.py
python SCRIPT_03B_DEFICIT_HIDRICO.py
python SCRIPT_03C_ESTRES_TERMICO.py
python SCRIPT_03D_SEQUIA.py
python SCRIPT_03E_HELADAS.py
python SCRIPT_03F_AGREGACION.py
python SCRIPT_04B_EXPOSICION.py
python SCRIPT_04C_DESAGREGACION.py
python SCRIPT_05A_GBIF.py
python SCRIPT_05B_DATASET_RF.py
python SCRIPT_05C_VARS_TERMICAS.py
python SCRIPT_06_RANDOM_FOREST.py
python SCRIPT_06B_PROYECCION_RF.py
python SCRIPT_06C_AGREGACION_PARR.py
python SCRIPT_07_RED_BAYESIANA.py
python SCRIPT_08_MAPAS.py
python SCRIPT_09_FICHAS.py
python SCRIPT_10_VALIDACION.py
```

Estimated total runtime: ~4–6 hours (dominated by Scripts 01–02 for CMIP6 data download).

---

## Key Results

| Metric | Value |
|--------|-------|
| Risk Index combinations | 1,512 (42 parishes × 4 crops × 3 SSPs × 3 horizons) |
| RF model AUC range | 0.804 – 0.871 |
| Highest-risk parish | García Moreno (IR = 0.689, SSP5-8.5, 2061–2080) |
| Most vulnerable crop | Potato (IR = 0.596) |
| Most climate-stable crop | Common bean (ΔIR = +0.6%) |
| Quinoa relative risk increase | +46.7% (SSP1-2.6 to SSP5-8.5) |
| Cartographic products | 58 maps |
| Parish technical sheets | 42 PDFs |

---

## Replicability for Other Provinces

The pipeline is designed for replication across Ecuadorian provinces. To adapt it:

1. Replace the GeoPackage in `config/config.json` with the target province's administrative boundaries
2. Update the bounding box coordinates
3. Download GBIF occurrences for the calibration region (Script 05A handles this automatically)
4. Run the full pipeline

BASD-CMIP6-PE covers all of Ecuador and Peru, so no changes to climate data sourcing are required.

---

## Citation

If you use this pipeline or data in your research, please cite:

```
Pinto Páez, V. H. (2026). Agroclimatic risk of Andean crops under CMIP6 scenarios 
in Imbabura, Ecuador: A reproducible Random Forest–Bayesian Network pipeline 
[Software]. Zenodo. https://doi.org/10.5281/zenodo.19022773
```

---

## License

This project is licensed under the MIT License — see the [LICENSE](LICENSE) file for details.

---

## Contact

**Víctor Hugo Pinto Páez**  
Maestría en Prevención y Gestión de Riesgos  
Universidad San Gregorio de Portoviejo, Ecuador  
📧 vpintopaez@hotmail.com

---

## References

- Fernandez-Palomino, C. A., et al. (2024). BASD-CMIP6-PE. *Scientific Data*, 11, 34. https://doi.org/10.1038/s41597-023-02863-z
- Breiman, L. (2001). Random Forests. *Machine Learning*, 45(1), 5–32.
- Pearl, J. (1988). *Probabilistic Reasoning in Intelligent Systems*. Morgan Kaufmann.
- IPCC. (2022). *Climate Change 2022: Impacts, Adaptation and Vulnerability*. Cambridge University Press.
- IFPRI. (2024). MapSPAM 2020 V2r0. Harvard Dataverse. https://doi.org/10.7910/DVN/SWPENT
