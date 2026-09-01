# HFpEF Phenotype Calculator

A bedside calculator that assigns patients with incident heart failure with
preserved ejection fraction (HFpEF) to one of five prognostically distinct
phenogroups, using 10 routine clinical variables.

**Live tool:** _(add the GitHub Pages URL here once deployed, e.g.
`https://<username>.github.io/hfpef-phenotype-calculator/`)_

## Background

The five phenogroups — Cardiorenal-severe, Hypertrophic-remodeling,
Elderly-frail, Obese-metabolic, and De-novo-low-risk — were derived via
3-round hierarchical unsupervised clustering on a 3-hospital Singapore
incident HFpEF cohort (N=4,289), and validated via bootstrap stability,
a second clustering algorithm (Gaussian mixture model), temporal
(2022–23 vs. 2024–25) split validation, and site-level validation.

This calculator implements a multinomial logistic-regression approximation of
that clustering, fit to reproduce phenogroup membership from 10 variables
that are already part of routine clinical workup:

- Age
- Sex
- eGFR (CKD-EPI 2021, race-free)
- BMI
- Haemoglobin
- Echocardiographic LV mass index
- Echocardiographic relative wall thickness (RWT)
- E/e'
- History of stroke
- History of COPD

Cross-validated performance (5-fold, N=3,685 with complete data):
**accuracy 82.4%, Cohen's κ = 0.776**, per-phenotype one-vs-rest AUC 0.95–0.99.

## How it works

For each phenotype, a linear score is computed from the 10 inputs. The five
scores are converted to probabilities via softmax, and the phenotype with the
highest probability is returned, along with a confidence tier:

- **High confidence** — top probability ≥ 80%
- **Moderate confidence** — top probability 60–80%
- **Low / indeterminate** — top probability < 60% (the tool flags this and
  names the two candidate phenotypes rather than forcing a single label)

All computation runs client-side in the browser — no patient data is
transmitted or stored anywhere.

## Important limitations

- This is a **screening/research aid, not a diagnostic tool**.
- Derived and cross-validated **only within a single 3-hospital Singapore
  cohort** — not yet externally validated in other populations or health
  systems.
- Precision is moderate (~50–75%) even where discrimination (AUC) is good;
  a given assignment should prompt clinical correlation, not replace it.
- Requires creatinine-derived eGFR (CKD-EPI 2021), not eGFR from another
  equation.

## Citation

_Manuscript citation to be added on publication._

## License

MIT — see [LICENSE](LICENSE).
