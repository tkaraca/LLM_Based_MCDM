# Auditable Multi-LLM Criterion Engineering and MCDM

This repository contains the manuscript-aligned, offline reproducibility package for:

> **An Auditable Multi-LLM Criterion Engineering Framework for Evidence-Grounded and Human-Benchmarked MCDM: Emergency Logistics Node Prioritisation in Istanbul**

The active pipeline reproduces the full frozen analysis from a 100-record multi-LLM criterion corpus to semantic family discovery, cross-source evidence fusion, public-data MCDM ranking, uncertainty analysis, and blind human benchmarking.

## Repository status

- Active framework code: included under `src/`
- Frozen reproducibility inputs: included under `data/`
- Regression-validation targets: included under `data/expected/`
- Selected reference outputs: included under `reference_outputs/`
- Complete generated outputs, figures, and legacy audit scripts: compressed under `release_assets/`
- Offline full reproduction: supported from the bundled frozen inputs
- Computational implementation: vectorised/reused hierarchies for practical full-run execution without changing frozen results
- Optional upstream geospatial regeneration: requires the external OSM snapshot described in the source manifest

The frozen full run previously completed with **25 PASS, 1 WARN, and 0 FAIL** validation checks. The warning concerns only the HDBSCAN reporting specification; it does not alter the locked codebook, criterion weights, public-data matrix, or final rankings.

## Quick start

### Python virtual environment

```bash
python -m venv .venv
```

Linux/macOS:

```bash
source .venv/bin/activate
pip install -r requirements.txt
python run_all.py
```

Windows PowerShell:

```powershell
.venv\Scripts\Activate.ps1
pip install -r requirements.txt
python run_all.py
```

### Conda

```bash
conda env create -f environment.yml
conda activate auditable-multillm-mcdm
python run_all.py
```

### Short smoke test

```bash
python run_all.py --quick
```

Quick mode reduces the resampling and Monte Carlo counts and therefore skips frozen-output validation.

## Computational stages

| Stage | Script | Main products |
|---|---|---|
| Criterion-family discovery | `src/01_criterion_family_discovery.py` | TF-IDF/SVD representation, agglomerative scan, resampling stability, HDBSCAN robustness, medoids, boundary cases |
| Evidence fusion and shortlist | `src/02_criterion_evidence_fusion.py` | Within-source MAX consolidation, family evidence channels, core/optional/hard-constraint/merge decisions |
| Public-data MCDM | `src/03_public_data_mcdm.py` | PROMETHEE II, TOPSIS, VIKOR, D1/D2 sensitivity, 5,000-run Monte Carlo rank acceptability |
| Human benchmark | `src/04_human_benchmark.py` | Panel aggregation, confidence weighting, Kendall's W, ICC(2,k), data-human concordance, hybrid rankings |
| Validation | `src/05_validate_reproduction.py` | PASS/WARN/FAIL comparison against frozen expected outputs |

## Frozen inputs

| Path | Purpose |
|---|---|
| `data/criterion/criterion_corpus.csv` | 100 raw criteria with source, description, parent dimension, direction, and available importance information |
| `data/criterion/raw_to_family_mapping.csv` | Frozen post-clustering researcher mapping |
| `data/criterion/approved_family_codebook.csv` | Frozen 26-family numerical codebook used by the reproducibility pipeline |
| `data/public/raw_performance.csv` | Candidate-level D1-D8 values and audit status |
| `data/public/candidate_registry.csv` | Initial and retained alternatives with audit metadata |
| `data/public/source_manifest.csv` | Provenance, source class, licence/status, and available checksums |
| `data/human/human_raw_scores.csv` | 240 blind expert ratings for D5, D7, and D8 |
| `data/expected/` | Frozen expected outputs used by the validation suite |

## Outputs

Running `python run_all.py` creates:

```text
outputs/01_criterion_discovery/
outputs/02_evidence_fusion/
outputs/03_public_data_mcdm/
outputs/04_human_benchmark/
outputs/validation/
figures/01_criterion_discovery/
```

Selected manuscript-facing reference outputs are already available in `reference_outputs/`. The complete previously generated output tree is retained as `release_assets/reference_outputs_full.zip`.

## Methodological safeguards

- Criterion importance is excluded from semantic family discovery.
- Word- and character-level TF-IDF are fitted to the full criterion corpus and reduced once into a shared latent space.
- Average-linkage agglomerative clustering uses cosine distance.
- Stability uses 85% subsampling without replacement.
- Approved codebook labels are used only after clustering for comparison and downstream handoff.
- Multiple criteria from one source mapped to the same family use `MAX(global weight)`, not a sum.
- Missing importance remains missing rather than being converted to zero.
- Hard constraints remain outside compensatory aggregation.
- D1 and D2 are combined into D12 while preserving their joint weight.
- Public-data ranking remains primary; human ratings are benchmark and sensitivity layers.

## Repository structure

```text
config/                 Frozen settings and seeds
data/                   Reproducibility inputs and expected validation targets
docs/                   Manuscript-code crosswalk
reference_outputs/      Selected readable reference results
release_assets/         Compressed full outputs, figures, and historical audit scripts
src/                    Active manuscript-aligned framework code
run_all.py              Cross-platform full workflow entry point
README_TR.md             Turkish usage summary
REPRODUCIBILITY_PROTOCOL.md
METHOD_ALIGNMENT.md
DATA_DICTIONARY.md
```

## GitHub upload

Extract the release ZIP first, then upload or push the **contents of the repository folder**. Do not rely on GitHub to unpack the ZIP inside a repository. Detailed instructions are provided in `GITHUB_UPLOAD_GUIDE.md`.

## Data and licensing boundary

No blanket licence has been assigned. Before public release, the authors should choose a code licence and verify redistribution permissions for each data source. OpenStreetMap-derived material remains subject to ODbL and source-specific terms apply to facility and official-data records. See `LICENSE_AND_DATA_NOTICE.md`.

## Known boundary

The exact prose version of every researcher-approved family definition and inclusion/exclusion rule was not present in the supplied frozen artifacts. The numerical mapping is complete and reproducible. `data/criterion/proposed_codebook_definition_completion.csv` is explicitly marked for author review and is not used in any numerical result.
