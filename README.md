# The Dark Figure of Cybercrime: Underreporting Patterns in Brazilian Phishing Incidents Based on CERT.br Data (2016–2025)

Replication repository for the article submitted to the *Revista Pesquisa e Planejamento Econômico* (Institute of Applied Economic Research — IPEA) special issue on the Economics of Crime in Brazil.

> **Citation:** Lavandoski da Silva, Luiz Gustavo. "The dark figure of cybercrime: underreporting patterns in Brazilian phishing incidents based on CERT.br data (2016–2025)." *Revista Pesquisa e Planejamento Econômico* (under review).

---

## Overview

This repository contains the data and scripts used to produce all empirical results, tables, and figures in the article. The analysis examines the monthly series of phishing notifications reported to CERT.br between January 2016 and December 2025 (120 observations) through three complementary econometric procedures: STL decomposition, endogenous structural break detection via PELT-Bai-Perron, and log-linear OLS regression with HAC standard errors.

---

## Repository Structure

```
dark-figure-phishing-brazil/
├── README.md
├── requirements.txt
├── data/
│   ├── raw/                        # Raw CERT.br .txt files (see Data Sources below)
│   └── serie_mensal.csv            # Consolidated monthly series (output of 01_parse.py)
├── scripts/
│   ├── 01_parse.py                 # Parses raw CERT.br files and builds serie_mensal.csv
│   └── 02_analise.py               # Full empirical analysis: STL, PELT, OLS, figures
└── outputs/
    ├── tabela1_descritivas_anuais.csv
    ├── tabela2_componente_sazonal.csv
    ├── tabela3_quebras_estruturais.csv
    ├── tabela4_regressao.csv
    └── tabela5_efeitos_sazonais.csv
```

---

## Data Sources

### Primary data: CERT.br

The monthly incident notification series is published in the public domain by CERT.br (NIC.br/CGI.br) and can be accessed at:

- https://stats.cert.br/incidentes/
- https://cert.br/stats/

For each year from 2016 to 2025, download the following files and place them in `data/raw/`:

| File | Contents |
|------|----------|
| `YYYY_incidentes-mensal.txt` | Monthly total incident counts |
| `YYYY_tipos-incidente-mensal.txt` | Incidents by category (DoS, Fraud, Intrusion, etc.) |
| `YYYY_tipos-fraude-mensal.txt` | Fraud subcategories (phishing, malware) |

Replace `YYYY` with each year (2016, 2017, ..., 2025). The files are plain text with whitespace-delimited columns and comment lines beginning with `#`.

If you prefer to skip parsing, the consolidated file `data/serie_mensal.csv` is already included in this repository and can be used directly as input for `02_analise.py`.

### Secondary sources

Secondary sources used for qualitative triangulation (Kaspersky, Febraban, DataSenado, CETIC.br, Seade) are cited in the article and are publicly available at the respective institutional websites. They are not incorporated into the econometric model.

---

## Reproducing the Analysis

### 1. Install dependencies

```bash
pip install -r requirements.txt
```

### 2. (Optional) Re-parse raw data

Only necessary if you want to rebuild `serie_mensal.csv` from the original CERT.br files:

```bash
python scripts/01_parse.py
```

The script expects raw files in `data/raw/` and writes `serie_mensal.csv` to `data/`.

### 3. Run the full analysis

```bash
python scripts/02_analise.py
```

This script reads `data/serie_mensal.csv` and produces:

- All five output tables in `outputs/`
- All figures in `figuras/` (PNG at 300 dpi and PDF)

The complete run takes approximately 30–60 seconds on a standard laptop.

---

## Requirements

```
pandas>=1.5
numpy>=1.23
statsmodels>=0.14
ruptures>=1.1
matplotlib>=3.6
scipy>=1.10
```

See `requirements.txt` for pinned versions used in the original analysis (Python 3.11).

---

## Key Results

| Finding | Value |
|---------|-------|
| Trend coefficient (monthly) | −0.0154 (p = 0.007) |
| Annualized decline | ≈ −16.9% per year |
| Endogenous structural break | September 2017 |
| Pre-break monthly mean | 6,796 notifications |
| Post-break monthly mean | 2,562 notifications |
| Adjusted R² (full model) | 0.477 |
| Pandemic dummy | −0.302 (p = 0.004) |

---

## License

The scripts in this repository are released under the MIT License. The CERT.br data is in the public domain. Secondary sources retain their original copyrights and are not reproduced here.

---

## Contact

Luiz Gustavo Lavandoski da Silva  
PhD Candidate, World Political Economy, Universidade Federal do ABC (UFABC)  
gustavo.lavandoski@ufabc.edu.br / gustavo.lavandoski@hotmail.com
