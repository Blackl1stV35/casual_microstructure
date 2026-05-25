# Causal Discovery Beyond Training Distribution in Financial Microstructure

**Paper:** *Causal Discovery Beyond Training Distribution in Financial Microstructure: A Quasi-Intervention Framework for Limit Order Book Dynamics*  
**Author:** Kanokphan Sirithienthong, Kasetsart University  
**Contact:** kanokphan.s@ku.th  
**SSRN:** [Social Science Research Network](https://papers.ssrn.com/sol3/papers.cfm?abstract_id=6828158)

---

## Overview

This repository contains all code, experiment notebooks, and LaTeX source for the paper. The central claim: a causal discovery framework operating on replayed market data via synthetic return injections strictly outperforms data-driven frameworks in out-of-distribution generalization — proved theoretically and validated empirically on 5.68 million XAUUSD M1 bars.

### Key results

| Result | Finding |
|--------|---------|
| Lemma 2 (causal depth) | $cd(\mathcal{L}) > 30$ M1 bars — XAUUSD causal memory exceeds Hasbrouck (1995) equity bound by 6× |
| Theorem 1 (quasi-do validity) | Synthetic wick-asymmetry injection is a valid Pearl do-operation — confirmed |
| Theorem 3 (Gödel horizon) | Dark-pool flow formally undiscoverable from M1 observable data |
| Theorem 4 (PSPACE) | Causal discovery in discrete financial time is PSPACE-complete |
| E3 (distributional shift) | LightGBM: Bear sell_P=0.903 → Bull sell_P=0.048 (−85.5pp); Causal-Structural: −45.8pp |

---

## Repository Structure

```
causal_microstructure/
├── experiments/
│   ├── E1_CausalDepth.ipynb          # Lemma 2: Granger depth, lags 1-30
│   ├── E2_QuasiDo.ipynb              # Theorems 1+2: structural quasi-do
│   └── E3_OOD_Generalization.ipynb   # Theorem 5: Bear→Bull shift
├── paper/
│   ├── causal_microstructure_jfe.tex # Main manuscript (double-blind)
│   ├── references.bib
│   ├── fig1.png                      # E1: Causal depth figure
│   ├── fig2.png                      # E2: Quasi-do certificate figure
│   └── fig3.png                      # E3: OOD generalization figure
├── README.md
└── LICENSE
```

---

## Data

**HFTExperiment NPZ** (`training_ready_v3b.npz`): 5,680,771 XAUUSD M1 bars, January 2009 – May 2026, 12-dimensional precomputed features. Constructed from publicly available XAUUSD M1 OHLCV data. The NPZ file is not included in this repository due to size (~2GB). Contact the author for access or see the data construction scripts in the [HFTExperiment repository](https://github.com/Blackl1stV35/HFTExperiment).

**Binance XAU/USDT L2 data** (preliminary experiments): freely accessible via the public REST and WebSocket API at `https://fapi.binance.com` — no authentication required.

---

## Reproducing the Experiments

All three experiments run on Google Colab T4 GPU (free tier). Each notebook is self-contained.

### Prerequisites
```
Google Colab T4 (12GB RAM)
training_ready_v3b.npz on Google Drive at:
  /content/drive/MyDrive/Colab Notebooks/training_ready_v3b.npz
```

### Run order

```
E1 → E3 → E2
```

E2 depends on E3's structural coefficients (`E3_results.pkl`). E3 depends on E1's causal depth result (`E1_results.pkl`).

### E1 — Causal Depth (~2 minutes)
Open `experiments/E1_CausalDepth.ipynb` in Colab. Run all cells.  
Output: `E1_results.pkl`, `fig_E1_causal_depth.pdf/png`

### E3 — OOD Generalization (~20 minutes)
Open `experiments/E3_OOD_Generalization.ipynb`. Run all cells.  
Output: `E3_results.pkl`, `fig_E3_ood_generalization.pdf/png`

### E2 — Quasi-Do Certificate (~15 minutes)
Open `experiments/E2_QuasiDo.ipynb`. Run all cells.  
Requires `E3_results.pkl` from the E3 run above.  
Output: `E2_results.pkl`, `fig_E2_quasi_do.pdf/png`

All outputs saved to `MyDrive/causal_microstructure/`.

---

## Theoretical Framework

Eight formal results established in the paper:

| Result | Statement |
|--------|-----------|
| **Lemma 1** | Faithfulness of M1 bar returns holds almost surely (algebraic geometry argument on compound queueing process) |
| **Lemma 2** | $cd(\mathcal{L}) > 30$ M1 bars for XAUUSD — empirically confirmed, no Granger edge decay through lag 30 |
| **Lemma 3** | Discovery operator terminates in finite iterations (rank reduction) |
| **Theorem 1** | Synthetic wick injection is a valid Pearl do-operation under 3 verifiable conditions |
| **Theorem 2** | Non-derivability certificate: structural impact function non-derivable from Kyle (1985) if nonlinear in $q$ |
| **Theorem 3** | Dark-pool flow formally undiscoverable from observable LOB data (Gödel horizon) |
| **Theorem 4** | Causal discovery in discrete financial time is PSPACE-complete |
| **Theorem 5** | Causal framework strictly dominates data-driven under distributional shift; gap monotone in shift magnitude |

---

## Axiom System Proved Incomplete

The paper proves the following standard microstructure axiom system $\mathcal{A}$ is incomplete:

- **Kyle (1985):** linear price impact $\Delta m = \lambda q$
- **Glosten–Milgrom (1985):** bid-ask spread $\sigma = 2\mu\pi/(1-\pi)$
- **Markov assumption:** $P(\Delta m_{t+1} \mid \mathcal{L}_t, \mathcal{L}_{t-1}, \ldots) = P(\Delta m_{t+1} \mid \mathcal{L}_t)$

Incompleteness: $cd(\mathcal{L}) > 30$ directly violates the Markov assumption, and the non-derivability certificate (Theorem 2) establishes mechanisms absent from $\mathcal{A}$.

---

## Citation

```bibtex
@techreport{sirithienthong2026causal,
  author      = {Sirithienthong, Kanokphan},
  title       = {Causal Discovery Beyond Training Distribution in Financial Microstructure: A Quasi-Intervention Framework for Limit Order Book Dynamics},
  institution = {Social Science Research Network (SSRN)},
  year        = {2026},
  type        = {Working Paper},
  number      = {6828158},
  url         = {https://papers.ssrn.com/sol3/papers.cfm?abstract_id=6828158},
  note        = {SSRN Scholarly Paper}
}
```

---

## Related Work

This paper is part of a broader research programme on causal discovery beyond training distribution:

- **ForgeWM Paper 1:** Sheaf-Cohomology-Guided Physical Anomaly Discovery (JMLR, under review)
- **ForgeWM Paper 2:** Exact Non-Derivability Certification via Causal Intervention (JMLR, under review)
- **ForgeWM Paper 3:** PSPACE-Completeness of Physical Law Discovery (JMLR, under review)

Repository: [https://github.com/Blackl1stV35/forgwm](https://github.com/Blackl1stV35/forgwm)

---

## License

MIT License — see `LICENSE` file.

---

*Kasetsart University · Department of Economics · Bangkok, Thailand · 2026*
