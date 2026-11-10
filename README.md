# lime-rec-synthesis

How much lime (calcium carbonate equivalent, CCE) do US liming recommendations prescribe for a given pH change?

## Data

The data are not in this repository. Download them from Zenodo
([doi:10.5281/zenodo.24117711](https://doi.org/10.5281/zenodo.24117711)) and put the files in `data/`, so that the
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
each file.

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

## License
This work is licensed under an MIT license. 

## About us
CarbonPlan is a nonprofit organization that uses data and science for climate action. We aim to improve the transparency and scientific integrity of climate solutions with open data and tools. Find out more at [carbonplan.org](https://carbonplan.org/) or get in touch by [opening an issue](https://github.com/carbonplan/soil-depth-sampling/issues/new) or [sending us an email](mailto:hello@carbonplan.org).
