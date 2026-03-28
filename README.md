# Riesgo Agroclimático de Cultivos Andinos bajo Escenarios CMIP6 en Imbabura

[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.19022773.svg)](https://doi.org/10.5281/zenodo.19022773)
[![ORCID](https://img.shields.io/badge/ORCID-0009--0001--5573--8294-green?logo=orcid)](https://orcid.org/0009-0001-5573-8294)
[![License: CC BY 4.0](https://img.shields.io/badge/License-CC%20BY%204.0-lightgrey.svg)](https://creativecommons.org/licenses/by/4.0/)

**Autor:** Víctor Hugo Pinto Páez  
**Institución:** Universidad San Gregorio de Portoviejo (USGP) — Maestría en Prevención y Gestión de Riesgos con Mención en Variabilidad Climática y Resiliencia Territorial  
**Año:** 2026  
**Contacto:** vpintopaez@hotmail.com

---

## Resumen

Este repositorio contiene el pipeline computacional completo (22 scripts Python, 10 fases) utilizado para evaluar el riesgo agroclimático de cuatro cultivos andinos — **papa** (*Solanum tuberosum*), **maíz** (*Zea mays*), **fréjol** (*Phaseolus vulgaris*) y **quinua** (*Chenopodium quinoa*) — en las 42 parroquias de la provincia de Imbabura, Ecuador, bajo los escenarios de cambio climático CMIP6 (SSP1-2.6, SSP3-7.0, SSP5-8.5) para los horizontes 2021–2040, 2041–2060 y 2061–2080.

El marco metodológico integra:
- **Modelos de Distribución de Especies (SDM)** con Random Forest sobre datos BASD-CMIP6-PE (Fernandez-Palomino et al., 2024)
- **Red Bayesiana de 7 nodos** para la integración de Peligro × Exposición × Vulnerabilidad (marco IPCC AR6)
- **1.512 valores de Índice de Riesgo** (IR), **58 mapas cartográficos** (300 DPI) y **42 fichas técnicas parroquiales**

Los datos completos (CSVs, mapas PDF en alta resolución, fichas parroquiales) están archivados en Zenodo: **[https://doi.org/10.5281/zenodo.19022773](https://doi.org/10.5281/zenodo.19022773)**

---

## Resultados Principales

| Indicador | Valor |
|-----------|-------|
| Parroquia con mayor riesgo | García Moreno — IR = 0,689 (SSP5-8.5, 2061–2080) |
| Parroquia con menor riesgo | Imbaya — IR = 0,350 |
| Cultivo más vulnerable | Papa — IR promedio ≈ 0,596 |
| Mayor incremento relativo | Quinua — +46,6% a +46,8% |
| Cultivo más estable | Fréjol — ΔIR ≤ +0,5% |
| AUC Random Forest | 0,804 – 0,871 (4 cultivos) |
| Spearman ρ (validación) | ≥ 0,91 |
| Superficie agrícola total | 10.706,1 ha |

---

## Estructura del Repositorio

```
agroclimatic-risk-imbabura/
│
├── README.md
├── LICENSE                          # CC BY 4.0
├── requirements.txt                 # Dependencias Python
├── config.json                      # Configuración de rutas del pipeline
│
├── scripts/                         # 22 scripts Python (10 fases)
│   ├── phase_00/
│   │   └── SCRIPT_00_CONFIGURACION_INICIAL_DEL_PROYECTO.py
│   ├── phase_01/
│   │   ├── SCRIPT_01_DESCARGA_Y_PROCESAMIENTO_DE_BASD-CMIP6-PE.py
│   │   └── SCRIPT_01B_DESCARGA_DE_DATOS_CLIMATICOS_BASD-CMIP6-PE.py
│   ├── phase_02/
│   │   └── SCRIPT_02_RECORTE_ESPACIAL_DE_BASD-CMIP6-PE_A_IMBABURA.py
│   ├── phase_03/
│   │   ├── SCRIPT_03A_CALCULO_DE_EVAPOTRANSPIRACION_DE_REFERENCIA.py
│   │   ├── SCRIPT_03B_CALCULO_DE_DEFICIT_HIDRICO_DIARIO.py
│   │   ├── SCRIPT_03C_CONTEO_DE_DIAS_CON_ESTRES_TERMICO_POR_CULTIVO.py
│   │   ├── SCRIPT_03D_DETECCION_DE_SEQUIAS_AGRICOLAS.py
│   │   ├── SCRIPT_03E_CONTEO_DE_HELADAS.py
│   │   └── SCRIPT_03F_AGREGACION_TEMPORAL_DE_INDICES_AGROCLIMATICOS.py
│   ├── phase_04/
│   │   ├── SCRIPT_04B_ADQUISICION_MULTI-FUENTE_DE_DATOS_DE_DISTRIBUCION.py
│   │   └── SCRIPT_04C_DESAGREGACION_ESPACIAL_DE_SUPERFICIE_AGRICOLA.py
│   ├── phase_05/
│   │   ├── SCRIPT_05A_v1_0_DESCARGA_Y_LIMPIEZA_DE_OCURRENCIAS_GBIF.py
│   │   ├── SCRIPT_05B_v2_0_EXTRACCION_CLIMATICA_PSEUDO-AUSENCIAS.py
│   │   └── SCRIPT_05C_AGREGAR_VARIABLES_TERMICAS_A_DATASETS_RF.py
│   ├── phase_06/
│   │   ├── SCRIPT_06_ENTRENAMIENTO_DE_RANDOM_FOREST_ANALISIS_SHAP.py
│   │   ├── SCRIPT_06B_v1_4_PROYECCION_DE_RANDOM_FOREST_A_CMIP6.py
│   │   └── SCRIPT_06C_AGREGACION_PARROQUIAL_NEAREST_NEIGHBOR.py
│   ├── phase_07/
│   │   └── SCRIPT_07_RED_BAYESIANA_INTEGRACION_RIESGO_AGROCLIMATICO.py
│   ├── phase_08/
│   │   └── SCRIPT_08_GENERACION_DE_MAPAS_DE_RIESGO_AGROCLIMATICO.py
│   ├── phase_09/
│   │   └── SCRIPT_09_GENERACION_DE_FICHAS_TECNICAS_PARROQUIALES.py
│   └── phase_10/
│       └── SCRIPT_10_VALIDACION_FINAL_Y_AUDITORIA_ISO_19115.py
│
├── data/
│   ├── results/                     # Resultados finales autoritativos
│   │   ├── riesgo_parroquial_20260226_172751.csv      # 1.512 valores IR ← CLAVE
│   │   ├── ranking_priorizacion_20260309_144736.csv   # Ranking 42 parroquias
│   │   └── exposicion_resumen_20260224_205600.csv     # Exposición agrícola
│   ├── training/                    # Datasets de entrenamiento RF
│   │   ├── dataset_rf_frejol_20260226_091606.csv      # 957 registros
│   │   ├── dataset_rf_maiz_20260226_091606.csv        # 2.062 registros
│   │   ├── dataset_rf_papa_20260226_091606.csv        # 902 registros
│   │   ├── dataset_rf_quinua_20260226_091606.csv      # 245 registros
│   │   └── dataset_rf_training_20260226_091606.csv    # 4.166 registros (combinado)
│   ├── occurrences/                 # Ocurrencias GBIF limpias (raw)
│   │   ├── ocurrencias_frejol_raw_20260225_073722.csv
│   │   ├── ocurrencias_maiz_raw_20260225_073722.csv
│   │   ├── ocurrencias_papa_raw_20260225_073722.csv
│   │   └── ocurrencias_quinua_raw_20260225_073722.csv
│   └── exposure/                   # Datos de exposición agrícola
│       ├── exposicion_parroquial_20260224_205600.csv
│       ├── quinua_provincial_20260224_205600.csv
│       └── INDICE_FICHAS_20260309_181550.csv
│
├── maps/                            # 58 mapas cartográficos (PNG 300 DPI)
│   ├── individual/                  # 36 mapas IR por cultivo × SSP × horizonte
│   │   ├── IR_frejol_ssp126_2021_2040.png
│   │   ├── IR_frejol_ssp126_2041_2060.png
│   │   ├── IR_frejol_ssp126_2061_2080.png
│   │   ├── IR_frejol_ssp370_2021_2040.png
│   │   ├── IR_frejol_ssp370_2041_2060.png
│   │   ├── IR_frejol_ssp370_2061_2080.png
│   │   ├── IR_frejol_ssp585_2021_2040.png
│   │   ├── IR_frejol_ssp585_2041_2060.png
│   │   ├── IR_frejol_ssp585_2061_2080.png
│   │   ├── IR_maiz_ssp126_2021_2040.png  ... [y 27 más]
│   │   ├── IR_papa_ssp126_2021_2040.png  ... [9 mapas papa]
│   │   └── IR_quinua_ssp126_2021_2040.png ... [9 mapas quinua]
│   ├── panels/                      # 8 paneles comparativos
│   │   ├── PANEL_IR_papa_9escenarios.png
│   │   ├── PANEL_IR_maiz_9escenarios.png
│   │   ├── PANEL_IR_frejol_9escenarios.png
│   │   ├── PANEL_IR_quinua_9escenarios.png
│   │   ├── PANEL_IR_ssp126_4cultivos.png
│   │   ├── PANEL_IR_ssp370_4cultivos.png
│   │   ├── PANEL_IR_ssp585_4cultivos.png
│   │   └── PANEL_RESUMEN_4cultivos_SSP585_20612080.png
│   ├── change/                      # 12 mapas de cambio temporal ΔIR
│   │   ├── CAMBIO_frejol_ssp126.png
│   │   ├── CAMBIO_frejol_ssp370.png
│   │   ├── CAMBIO_frejol_ssp585.png
│   │   ├── CAMBIO_maiz_ssp126.png  ... [y 9 más]
│   │   └── CAMBIO_quinua_ssp585.png
│   └── synthesis/                   # 2 mapas de síntesis
│       ├── EXPOSICION_superficie_parroquial.png
│       └── SINTESIS_priorizacion_parroquias.png
│
├── technical_sheets/                # 42 fichas técnicas parroquiales (PDF)
│   ├── FICHA_ANTONIO_ANTE_ATUNTAQUI.pdf
│   ├── FICHA_ANTONIO_ANTE_IMBAYA.pdf
│   ├── FICHA_ANTONIO_ANTE_SAN_FRANCISCO_DE_NATABUELA.pdf
│   ├── FICHA_ANTONIO_ANTE_SAN_JOSE_DE_CHALTURA.pdf
│   ├── FICHA_ANTONIO_ANTE_SAN_ROQUE.pdf
│   ├── FICHA_COTACACHI_APUELA.pdf
│   ├── FICHA_COTACACHI_COTACACHI.pdf
│   ├── FICHA_COTACACHI_GARCIA_MORENO.pdf
│   ├── FICHA_COTACACHI_IMANTAG.pdf
│   ├── FICHA_COTACACHI_PENAHERRERA.pdf
│   ├── FICHA_COTACACHI_PLAZA_GUTIERREZ.pdf
│   ├── FICHA_COTACACHI_QUIROGA.pdf
│   ├── FICHA_COTACACHI_SEIS_DE_JULIO_DE_CUELLAJE.pdf
│   ├── FICHA_COTACACHI_VACAS_GALINDO.pdf
│   ├── FICHA_IBARRA_AMBUQUI.pdf
│   ├── FICHA_IBARRA_ANGOCHAGUA.pdf
│   ├── FICHA_IBARRA_LA_CAROLINA.pdf
│   ├── FICHA_IBARRA_LA_ESPERANZA.pdf
│   ├── FICHA_IBARRA_LITA.pdf
│   ├── FICHA_IBARRA_SALINAS.pdf
│   ├── FICHA_IBARRA_SAN_ANTONIO.pdf
│   ├── FICHA_IBARRA_SAN_MIGUEL_DE_IBARRA.pdf
│   ├── FICHA_OTAVALO_DR_MIGUEL_EGAS_CABEZAS.pdf
│   ├── FICHA_OTAVALO_EUGENIO_ESPEJO.pdf
│   ├── FICHA_OTAVALO_GONZALEZ_SUAREZ.pdf
│   ├── FICHA_OTAVALO_OTAVALO.pdf
│   ├── FICHA_OTAVALO_PATAQUI.pdf
│   ├── FICHA_OTAVALO_SAN_JOSE_DE_QUICHINCHE.pdf
│   ├── FICHA_OTAVALO_SAN_JUAN_DE_ILUMAN.pdf
│   ├── FICHA_OTAVALO_SAN_PABLO.pdf
│   ├── FICHA_OTAVALO_SAN_RAFAEL.pdf
│   ├── FICHA_OTAVALO_SELVA_ALEGRE.pdf
│   ├── FICHA_PIMAMPIRO_CHUGA.pdf
│   ├── FICHA_PIMAMPIRO_MARIANO_ACOSTA.pdf
│   ├── FICHA_PIMAMPIRO_PIMAMPIRO.pdf
│   ├── FICHA_PIMAMPIRO_SAN_FRANCISCO_DE_SIGSIPAMBA.pdf
│   ├── FICHA_SAN_MIGUEL_DE_URCUQUI_CAHUASQUI.pdf
│   ├── FICHA_SAN_MIGUEL_DE_URCUQUI_LA_MERCED_DE_BUENOS_AIRES.pdf
│   ├── FICHA_SAN_MIGUEL_DE_URCUQUI_PABLO_ARENAS.pdf
│   ├── FICHA_SAN_MIGUEL_DE_URCUQUI_SAN_BLAS.pdf
│   ├── FICHA_SAN_MIGUEL_DE_URCUQUI_TUMBABIRO.pdf
│   └── FICHA_SAN_MIGUEL_DE_URCUQUI_URCUQUI.pdf
│
└── metadata/                        # Metadatos y auditoría
    ├── metadatos_iso19115_v3_20260224_205600.json     # ISO 19115 autoritativo
    ├── comparacion_fuentes_20260224_205600.json
    └── REPORTE_AUDITORIA_04C_v3_20260224_205600.txt
```

> **Nota:** Los mapas en alta resolución en formato PDF y los documentos metodológicos completos (21 archivos DOCX) están disponibles exclusivamente en el archivo Zenodo: [https://doi.org/10.5281/zenodo.19022773](https://doi.org/10.5281/zenodo.19022773)

---

## Pipeline — 10 Fases, 22 Scripts

| Fase | Script(s) | Descripción |
|------|-----------|-------------|
| 00 | `SCRIPT_00` | Configuración inicial, estructura de directorios |
| 01 | `SCRIPT_01`, `SCRIPT_01B` | Descarga y procesamiento BASD-CMIP6-PE |
| 02 | `SCRIPT_02` | Recorte espacial a Imbabura |
| 03 | `SCRIPT_03A`–`03F` | Cálculo de 16 índices agroclimáticos diarios |
| 04 | `SCRIPT_04B`, `04C` | Exposición agrícola (MapSPAM + ESPAC 2024) |
| 05 | `SCRIPT_05A`–`05C` | Ocurrencias GBIF + pseudo-ausencias + dataset RF |
| 06 | `SCRIPT_06`, `06B`, `06C` | Entrenamiento RF, proyección CMIP6, agregación parroquial |
| 07 | `SCRIPT_07` | Red Bayesiana — Índice de Riesgo (1.512 valores) |
| 08 | `SCRIPT_08` | 58 mapas cartográficos (300 DPI) |
| 09 | `SCRIPT_09` | 42 fichas técnicas parroquiales PDF |
| 10 | `SCRIPT_10` | Validación final y auditoría ISO 19115 |

---

## Datos Climáticos — BASD-CMIP6-PE

| Parámetro | Valor |
|-----------|-------|
| Fuente | Fernandez-Palomino et al. (2024) — *Scientific Data* 11, 34 |
| Resolución espacial | ~10 km (0,1°) |
| Resolución temporal | Diaria |
| Período histórico | 1981–2014 |
| Período futuro | 2015–2100 |
| Número de GCMs | 10 |
| Escenarios SSP | SSP1-2.6 / SSP3-7.0 / SSP5-8.5 |
| Corrección de sesgo | ISIMIP3BASD v2.5 |
| Horizontes evaluados | 2021–2040 / 2041–2060 / 2061–2080 |

---

## Instalación y Reproducibilidad

```bash
# 1. Clonar repositorio
git clone https://github.com/VICTORSIG1985/agroclimatic-risk-imbabura.git
cd agroclimatic-risk-imbabura

# 2. Instalar dependencias
pip install -r requirements.txt

# 3. Configurar rutas en config.json
# Editar BASE_DIR según su sistema de archivos

# 4. Ejecutar pipeline secuencialmente
python scripts/phase_00/SCRIPT_00_CONFIGURACION_INICIAL_DEL_PROYECTO.py
python scripts/phase_01/SCRIPT_01_DESCARGA_Y_PROCESAMIENTO_DE_BASD-CMIP6-PE.py
# ... continuar en orden de fase
python scripts/phase_10/SCRIPT_10_VALIDACION_FINAL_Y_AUDITORIA_ISO_19115.py
```

**Requisitos:** Python ≥ 3.10 | Ver `requirements.txt` para versiones exactas de: scikit-learn, pgmpy, geopandas, rasterio, numpy, pandas, matplotlib, scipy.

---

## Limitaciones del Pipeline

Se documentan honestamente las siguientes limitaciones metodológicas:

1. **Resolución espacial (~10 km):** no captura microclimas sub-parroquiales
2. **MapSPAM como proxy:** no equivale a catastro agrícola parroquial real
3. **Quinua solo a nivel provincial:** evaluación exploratoria (no parroquial)
4. **Sin validación de campo:** los IR son rankings prospectivos de priorización, no predicciones determinísticas
5. **Conservatismo de nicho:** supone estacionariedad del nicho ecológico
6. **Vulnerabilidad como proxy operacional:** de la dimensión bioclimática únicamente; excluye capacidad adaptativa y factores socioinstitucionales

---

## Cita

```bibtex
@dataset{pinto_paez_2026,
  author       = {Pinto Páez, Víctor Hugo},
  title        = {Riesgo agroclimático de cultivos andinos bajo escenarios 
                  CMIP6 en la provincia de Imbabura: pipeline computacional, 
                  datos y productos cartográficos},
  year         = {2026},
  publisher    = {Zenodo},
  doi          = {10.5281/zenodo.19022773},
  url          = {https://doi.org/10.5281/zenodo.19022773},
  orcid        = {0009-0001-5573-8294}
}
```

---

## Referencia Principal

Fernandez-Palomino, C. A., Hattermann, F. F., Krysanova, V., Vega-Jácome, F., Menz, C., Gleixner, S. y Bronstert, A. (2024). High-resolution climate projection dataset based on CMIP6 for Peru and Ecuador: BASD-CMIP6-PE. *Scientific Data*, 11, 34. https://doi.org/10.1038/s41597-023-02863-z

---

## Licencia

Los datos y productos cartográficos se distribuyen bajo licencia **Creative Commons Attribution 4.0 International (CC BY 4.0)**.  
El código fuente se distribuye bajo licencia **MIT**.

Al utilizar estos materiales, cite: Pinto Páez (2026), DOI: [10.5281/zenodo.19022773](https://doi.org/10.5281/zenodo.19022773)
