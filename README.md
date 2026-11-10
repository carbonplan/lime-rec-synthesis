# lime-rec-synthesis

How much lime (calcium carbonate equivalent, CCE) do US liming recommendations prescribe for a given pH change?

## Data

The data are not in this repository. Download them from Zenodo
([doi:10.5281/zenodo.XXXXXXX](https://doi.org/10.5281/zenodo.XXXXXXX)) and put the files in `data/`, so that the
layout is:

```
data/
├── lime-recs.xlsx
├── recs_harmonized.csv
├── limer_harmonized.csv
├── max_dose.csv
└── external/tsao2025/county_lime_1954_1987.csv
```

`lime-recs.xlsx` and the Tsao et al. county table are the inputs; the other three CSVs are outputs of notebooks
01, 02 and 04, archived so they can be used without running the code. The `README.md` in the deposit describes
every file and column.

## Contents

| | |
|---|---|
| `notebooks/01_harmonize_recs.ipynb` | Combines the sheets of `data/lime-recs.xlsx` (hand-compiled recommendations, one sheet per state) into one table on a common dose basis → `data/recs_harmonized.csv` |
| `notebooks/02_limer.ipynb` | Builds the same table from the incubation data shipped with the [`limer`](https://github.com/cropmodels/limer) R package → `data/limer_harmonized.csv` |
| `notebooks/03_explore.ipynb` | Exploratory plots of lime dose vs. ΔpH |
| `notebooks/04_max_dose.ipynb` | Stated maximum lime doses by state, with where to find each in its source → `data/max_dose.csv` |
| `notebooks/05_lime_frequency.ipynb` | Lime recurrence intervals from county liming data (Tsao et al., [Zenodo 15758275](https://doi.org/10.5281/zenodo.15758275)) |
| `notebooks/06_figures.ipynb` | Figures comparing liming recommendations with the Beerling et al. (2025) 1 Gt case (reads `point.zarr` from S3, as `beerling-2025-analysis` does; needs AWS access or `$BEERLING_POSTPROC`) |

## Usage

```bash
pixi install
pixi run run-all   # executes notebooks 01–04 in order
pixi run lab       # or open them interactively
```

`02_limer` downloads its inputs from GitHub; the other notebooks read `data/`.

## Conventions

- **Dose** is converted to kg CCE ha⁻¹ (`dose_native` × unit factor × `cce_factor`). It is also given per soil mass
  (mmol_c kg⁻¹ of CaCO₃), using each source's mixing depth and a bulk density of 1.3 g cm⁻³ where none is given
  (`bd_assumed`).
- `co2_eq_t_ha` is the same dose as t CO₂-eq ha⁻¹: the CO₂ that the released cations could charge-balance as
  bicarbonate (each Ca²⁺ balances 2 HCO₃⁻, so × 2 × 44.01 / 100.09 = 0.88 t CO₂ per t CCE).
- **ΔpH** is `ph_target − ph_initial` when the source gives an initial pH (`dpH_basis = initial_pH`). Otherwise the
  ΔpH comes from differences between targets in the same soil: anchored at the lowest target, with the dose taken
  as the difference from the anchor dose (`dpH_basis = target_difference`). The two bases cover different parts
  of the pH range (field pH → target vs. roughly 6 → 7), and recommendations aren't linear in pH, so
  `03_explore` keeps them as separate series.
- `dose_dpH_*` columns hold the dose that goes with `dpH`.
