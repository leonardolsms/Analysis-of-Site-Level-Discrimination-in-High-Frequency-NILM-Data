# Analysis of Site-Level Discrimination in High-Frequency NILM Data: Acquisition Kit and Load Composition in WHITED v1.1

[![Workshop](https://img.shields.io/badge/WCNPS-2026-blue)](#citation)
[![Python](https://img.shields.io/badge/Python-3.x-3776AB?logo=python&logoColor=white)](#requirements)
[![scikit-learn](https://img.shields.io/badge/scikit--learn-1.6.1-F7931E?logo=scikitlearn&logoColor=white)](#requirements)
[![License: MIT](https://img.shields.io/badge/code-MIT-green)](#license)
<!-- Open in Colab: edit the path to the notebook inside the repository -->
[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/leonardolsms/Analysis-of-Site-Level-Discrimination-in-High-Frequency-NILM-Data/blob/main/notebooks/Projeto_WCNPS_26_Auditoria_versao_Final_semR9_.ipynb)

Code, notebook and supplementary material for the paper submitted at the **11th Workshop on Communication Networks and Power Systems (WCNPS 2026)**.

> **Authors:** Leonardo Santiago Melgaço Silva, Gabriel Arquelau Pimenta Rodrigues, Mariana Cabral Falqueiro, Geraldo Pereira Rocha Filho, André Luiz Marques Serrano, Vinícius Pereira Gonçalves
> **Affiliations:** University of Brasília (UnB), Federal Institute of Brasília (IFB), State University of Southwest Bahia (UESB)

---

## Contents

1. [Overview](#overview)
2. [Main findings](#main-findings)
3. [Repository structure](#repository-structure)
4. [Data](#data)
5. [Requirements](#requirements)
6. [How to reproduce](#how-to-reproduce)
7. [Mapping between the paper and the notebook](#mapping-between-the-paper-and-the-notebook)
8. [Methods at a glance](#methods-at-a-glance)
9. [Limitations](#limitations)
10. [Citation](#citation)
11. [License](#license)
12. [Contact](#contact)

---

## Overview

Non-Intrusive Load Monitoring (NILM) datasets are often assembled from different sites, acquisition kits and recording sessions. A classifier trained on such data can exploit properties that are confounded with the label (dataset bias, shortcut learning).

This project **audits WHITED v1.1** (Worldwide Household and Industry Transient Energy Data Set) and asks whether the ability of 12 waveform descriptors to predict the **measurement site** can be separated from:

- the **acquisition kit** (MK1 / MK2),
- the **electrical grid** (mains frequency, voltage channel),
- **signal gain** (amplitude-dependent descriptors),
- **appliance type / load composition**, and
- the **recording period**.

The study distinguishes *predictive discrimination* (features predict the site) from a *geographic electrical signature* (the information is a property of the location, such as its grid).

## Main findings

After removing duplicated files and one site outside the official v1.1 count (`r9`), the analysis set contains **1,259 observations from 126 appliances, 8 measurement sites and 2 kits**.

| Question | Result |
|---|---|
| Natural site clusters? | Site Silhouette **−0.0570**, above its permutation baseline (mean −0.198) but no well-separated global clusters; positive structure only in `r7` and `r8` |
| Kit structure | Kit Silhouette **0.2430** (null mean 0.002); driven by MK2; falls to 0.1411 with gain-independent descriptors |
| Site classification (Random Forest, appliance-grouped 5-fold CV) | **72.1 %** accuracy (71.5 % balanced); **78.3 %** (78.6 %) with the kit added |
| Kit gain over 10 partitions | **+4.5 ± 1.2** percentage points globally; **−0.7 ± 1.2** within the three sites shared by both kits (`r1`–`r3`) |
| Entire appliance types held out | 68.0 % accuracy (majority baseline 23.0 %) |
| Kit recoverability in `r1`–`r3` | Predicted with accuracy 1.00 (majority 0.794), mainly through current descriptors |
| Grid | Only `r8` operates at 60 Hz (60.02 ± 0.02 Hz); voltage-only descriptors perform at the majority baseline |

**Conclusion.** The data contain site-associated predictive information, but they do **not** establish an independent geographic electrical signature: site, kit, recording period and load composition remain confounded.

## Repository structure

<!-- Adjust to the actual layout of the repository -->

```text
.
├── README.md
├── LICENSE
├── CITATION.cff
├── requirements.txt
├── notebooks/
│   └── Projeto_WCNPS_26_Auditoria_versao_Final_semR9_.ipynb   # full pipeline (Google Colab)
├── figures/                  # figures used in the paper (Fig. 1, Fig. 2, confusion matrix, ...)
├── tables/                   # CSV exports of the paper tables
├── paper/
│   └── ARTICLE_SBC_WCNPS_2026.pdf
└── docs/
    └── (optional) supplementary material
```

## Data

The **WHITED v1.1 package is not redistributed** in this repository. Download it from the official page and follow the terms stated there:

- Technical University of Munich, *WHITED – Worldwide Household and Industry Transient Energy Data Set, version 1.1*: <https://www.cs.cit.tum.de/dis/resources/whited/>
- Original dataset paper: M. Kahl, A. U. Haq, T. Kriechbaumer and H.-A. Jacobsen, "WHITED – a worldwide household and industry transient energy data set," *3rd International Workshop on Non-Intrusive Load Monitoring (NILM)*, Vancouver, 2016.

Each recording is a 5 s, 44.1 kHz, two-channel FLAC file (left: current; right: voltage) named `[Class]_[Name]_[Region]_[Kit]_[TimeStamp].flac`.

**Data audit performed by the notebook**

| Step | Count |
|---|---|
| FLAC files in the package | 5,016 |
| Files following the pattern `<appliance>_<site>_<kit>_<timestamp>` | 3,926 |
| Files without site/kit tags (excluded) | 1,090 |
| Distinct recordings after filename-based deduplication | 1,339 |
| Site `r9` (80 recordings, 8 appliances, May 2017; absent from the official count; excluded) | −80 |
| **Final analysis set** | **1,259 observations / 126 appliances** |

No two distinct filenames had identical audio content (MD5) or identical feature vectors.

## Requirements

The analysis runs in **Google Colab** (recommended) or any local Jupyter environment.

- Python 3
- `scikit-learn==1.6.1` (version used in the paper)
- `numpy`, `pandas`, `scipy`, `statsmodels`, `soundfile`, `matplotlib`, `jupyter`

<!-- Replace with the exact versions printed by `!pip freeze` in the Colab session used for the paper -->

```bash
pip install numpy pandas scipy statsmodels soundfile matplotlib jupyter scikit-learn==1.6.1
```

## How to reproduce

### Option A — Google Colab

1. Click the **Open in Colab** badge above (or upload the notebook from `notebooks/`).
2. Download WHITED v1.1 from the TUM page and upload/extract it in the Colab session. The notebook expects the extracted package at `/content/WHITED_v1_1/` (edit the path variable in the first cells if you use another location).
3. Run **Runtime → Run all**. The final cells export tables (CSV) and figures.

### Option B — Local Jupyter

```bash
git clone https://github.com/leonardolsms/Analysis-of-Site-Level-Discrimination-in-High-Frequency-NILM-Data.git
cd Analysis-of-Site-Level-Discrimination-in-High-Frequency-NILM-Data
pip install -r requirements.txt
jupyter notebook notebooks/Projeto_WCNPS_26_Auditoria_versao_Final_semR9_.ipynb
```

Edit the dataset path at the top of the notebook before running.

### Reproducibility settings

| Item | Setting |
|---|---|
| Classifier | Random Forest, 500 trees, `class_weight="balanced"` |
| Validation | 5-fold `StratifiedGroupKFold` (shuffle), grouping by **appliance** |
| Reference partition | seed 42 (Tables III and IV) |
| Stability analysis | seeds 0–9, models paired on the same partitions |
| Silhouette null | 1,000 label permutations at the appliance level (seed 0) |
| Descriptive analyses | median per appliance, z-scored features, Euclidean distance |
| Multiple testing | Benjamini–Hochberg FDR over the 12 features |

> The notebook should run from start to finish without errors in a fresh session. If a cell fails because a variable is undefined, run the cells in order from the top.

## Mapping between the paper and the notebook

| Paper | Content | Notebook block |
|---|---|---|
| Section III-A, Table I | Dataset audit, deduplication, site/kit counts | Audit cells; blocks **B1–B2** (r9 provenance, copies, MD5) |
| Section III-B | 12 descriptors (RMS, std, peak, crest factor, spectral centroid and bandwidth, per channel) | Feature extraction cells |
| Section IV-A, Fig. 1 | PCA of appliance medians | PCA cells |
| Section IV-B, Table II, Fig. 2 | Silhouette + permutation null | Silhouette cells; **B4**, **B6**, **B9b** |
| Section IV-C | Kruskal–Wallis, η²<sub>H</sub> | Statistics cells |
| Section IV-D, Table III | Random Forest, kit gain over 10 seeds | Classification cells; **B0**, **B6b**, **B8** |
| Section IV-E, Table IV | Mains frequency, current/voltage only, gain-independent, type-grouped CV, kit predictability | **B3**, **B4**, **B5**, **B9a**, **B10** |
| Section VII | Reproducibility | This repository |

Blocks `B0`–`B10` are the cells labelled `# ===== Bn — ... =====` in the notebook.

## Methods at a glance

- **Descriptors (12):** for each channel, RMS, standard deviation, peak amplitude and crest factor over the full 5 s, plus spectral centroid and bandwidth from the Hann-windowed FFT of the first 8,192 samples (~0.19 s). `f1`–`f6`: current; `f7`–`f12`: voltage. Amplitudes stay in normalized full-scale units (no calibration to A/V was applied).
- **Structure:** PCA and Silhouette coefficient at the appliance level, compared with permutation nulls.
- **Statistics:** Kruskal–Wallis on appliance medians with FDR control; effect size η²<sub>H</sub> = (H − k + 1) / (N − k); Cramér's V for site × appliance type.
- **Supervised evaluation:** Random Forest with appliance-grouped cross-validation; majority-class baselines; pooled and fold-mean metrics.
- **Sensitivity analyses:** mains frequency from the voltage channel; current-only vs. voltage-only; gain-independent descriptors; cross-validation grouped by appliance type; types present at several sites; kit classification within shared sites.

## Limitations

- Five of eight sites were recorded with a single kit, sites differ in recording period, and site and appliance type are strongly associated: **site, kit, session and load composition cannot be fully separated** with these data.
- The effective sample is closer to 126 appliances than to 1,259 recordings; some sites have only 5–7 appliances.
- The 12 descriptors are partly redundant, amplitudes are uncalibrated, and none encodes the mains frequency.
- One dataset, one classifier, default hyperparameters.
- The effect of these confounders on **appliance recognition** was not evaluated.

<!--## Citation

If you use this code or the results, please cite the paper:

```bibtex
@inproceedings{silva2026sitelevel,
  author    = {Silva, Leonardo Santiago Melga{\c{c}}o and Rodrigues, Gabriel Arquelau Pimenta and
               Falqueiro, Mariana Cabral and Rocha Filho, Geraldo Pereira and
               Serrano, Andr{\'e} Luiz Marques and Gon{\c{c}}alves, Vin{\'\i}cius Pereira},
  title     = {Analysis of Site-Level Discrimination in High-Frequency {NILM} Data:
               Acquisition Kit and Load Composition in {WHITED} v1.1},
  booktitle = {11th Workshop on Communication Networks and Power Systems (WCNPS 2026)},
  year      = {2026}
}
```

<!-- Update with pages / DOI when the proceedings are published. -->

GitHub also reads the `CITATION.cff` file in this repository ("Cite this repository").-->

## License

<!-- Choose before publishing and add the matching LICENSE file -->

- **Code:** MIT License (see [`LICENSE`](LICENSE)).
- **Text and figures:** CC BY 4.0 (optional).
- **WHITED data:** governed by the terms of the TUM dataset page; not included here.

## Contact

Leonardo Santiago Melgaço Silva — <contact-leonardo.santiago@aluno.unb.br> or <leonardosantiago.lsms@gmail.com>

Issues and questions are welcome through the [GitHub Issues](../../issues) tab.

---

<details>
<summary><b>Resumo em português</b></summary>

Este repositório reúne o código, o notebook (Google Colab) e o material suplementar do artigo submetido ao **WCNPS 2026**. O trabalho audita o conjunto de dados **WHITED v1.1** e investiga se a capacidade de 12 descritores de forma de onda de prever o **local de medição** pode ser separada do kit de aquisição, da rede elétrica, do ganho do sinal e da composição de carga.

Após a remoção de arquivos duplicados e de um local fora da contagem oficial (`r9`), foram analisadas **1.259 observações de 126 aparelhos, em 8 locais e 2 kits**. Um Random Forest com validação cruzada agrupada por aparelho obteve **72,1 %** de acurácia (71,5 % balanceada), subindo para **78,3 %** com a informação do kit; o ganho do kit foi de **4,5 ± 1,2 pontos percentuais** em dez partições e **nulo** nos três locais compartilhados pelos dois kits.

Os resultados indicam informação preditiva associada ao local, mas **não** estabelecem uma assinatura elétrica geográfica independente: local, kit, período de gravação e composição de carga permanecem confundidos.

Os dados do WHITED **não** estão incluídos; baixe-os na página da TUM (link na seção [Data](#data)) e ajuste o caminho no início do notebook.

</details>
