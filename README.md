# European Semiconductor Atlas

Open, sourced data on Europe's semiconductor supply chain: wafer fabs, back-end
(assembly, test and packaging) sites and equipment makers in the EU-27, the UK,
Switzerland and Norway. Every record carries source URLs and a verification date.

[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.23864138.svg)](https://doi.org/10.5281/zenodo.23864138)

**Release 2026.10.1** · data verified up to 2026-10-26 · licence
[CC BY 4.0](LICENSE)

Interactive map, searchable list and write-ups:
[richard-stahl.at/atlas/map/](https://richard-stahl.at/atlas/map/)

## Contents

| File | Records | What it is |
|---|---|---|
| `data/european-fabs.geojson` | 127 fab records at 101 operator campuses | Chip fabs, research and pilot lines, and wafer plants, with status, process, ownership, funding and capacity |
| `data/european-fabs.csv` | 127 | The fab layer as a flat table |
| `data/european-backend.geojson` | 44 sites | Assembly, test and packaging: captive plants, merchant services and research lines |
| `data/european-equipment.geojson` | 89 sites | Equipment makers and suppliers of parts for semiconductor production |
| `data/eu-grants.json` | 54 operators | EU research-grant contributions per operator from CORDIS (Horizon 2020 and Horizon Europe) |
| `data/eu-grants-projects.json` | 8623 participations | The project-level CORDIS evidence behind `eu-grants.json` |

Fab records by category: `volume_specialty` 34, `rd_pilot` 31, `substrate_epi` 22, `compound_semi` 21, `foundry` 14, `volume_logic` 5.
Fab records by status: `operational` 74, `expanding` 24, `rd_pilot` 13, `announced` 6, `under_construction` 5, `cancelled` 4, `closed` 1.
Countries with at least one fab record: 16.

## Fields

All three GeoJSON layers share a core: `id`, `company`, `site_city`, `country`,
`country_iso`, `in_eu`, `lat`, `lon`, `coord_precision`, `status`, `notes`,
`sources` (list of URLs), `confidence` (high / medium / low), `last_verified`
and a plain-language `summary`.

**Fab layer**

- `category`: one primary role. When several apply, the first in this order wins:
  `rd_pilot`, `substrate_epi`, `foundry`, `compound_semi`, `memory`,
  `volume_logic`, `volume_specialty`.
- Facets, each a closed list, derived from the record text:
  - `materials`: Si, SiC, GaN, GaAs, InP, Other
  - `functions`: Digital, Memory, Analog/mixed-signal, Power, RF, Sensing, Optoelectronics, Other
  - `form`: Integrated circuit, Discrete, MEMS, Passive, Wafer
  - `process`: the ten process families explained at
    [/atlas/chip-processes-explained/](https://richard-stahl.at/atlas/chip-processes-explained/)

  An empty facet means the record does not say.
- `owner`, `subsidy` and `capacity` are objects with their own `source`, `as_of`
  date and `verified` state.
- `wafer_size_mm`, `process_nodes`, `finest_node`, `finest_node_nm`,
  `year_opened` (first production, never a purchase or expansion year),
  `year_first_production`, `expected_production_year`, `employees` (site
  figure only), `investment_eur_million`, `site_id`.

**Units.** Capacity is always wafers per month. Investment and subsidy amounts
are always EUR millions. Where a source published another unit or currency, the
original figure is kept in `capacity.reported` or `investment_reported`, with
the exchange rate and its source.

**Missing values.** `gaps` records why a field is empty: `not_applicable`
(cannot apply), or `not_found` (a dated, documented search found no published
figure). Any other empty field has not been researched yet. No value is
estimated to fill a gap.

## Read before you use it

- **Funding amounts are recorded approvals, not payments.** `subsidy` holds the
  approved aid ceiling or grant amount from the cited decision.
  `subsidy.amount_scope` says whether it applies to the site, the company or a
  shared programme. Only site-scoped grants should be summed per site.
- **EU research grants are per operator, never per site.** `eu-grants.json`
  sums CORDIS EU contributions across an operator's legal entities (2014–2026).
  Do not assign them to fabs or countries.
- **Coordinates vary in precision.** `coord_precision` is `site`, `city` or
  `region`. A coordinate audit is in progress, and some site positions will
  move in later releases.
- **Coverage is what could be sourced.** Absence from the data is not evidence
  that a site does not exist.

## How it was built

Records are compiled from primary sources wherever they exist: operator reports
and press releases, regulatory filings, and European Commission state-aid
decisions. Discovery and first drafts used AI-assisted research agents. A
separate verification pass then fetched each cited source and checked the
claim against it, and I reviewed the disputed cases by hand. Each record keeps
its sources, so you can check any value yourself. Records whose source could
not be confirmed say so in their `verified` state or notes. More on how I use
AI: [richard-stahl.at/ai-transparency/](https://richard-stahl.at/ai-transparency/).

The atlas is updated quarterly. Each release is described in a changelog post on
[richard-stahl.at/atlas/](https://richard-stahl.at/atlas/). This release:
[New Chip Fabs in Europe: Project Status and Funding, October 2026](https://richard-stahl.at/atlas/european-chip-fab-projects/).

## Cite

> Stahl, R. (2026). *European Semiconductor Atlas* (Version 2026.10.1) [Data set]. https://doi.org/10.5281/zenodo.23864138

See also [`CITATION.cff`](CITATION.cff).

## Licence and attribution

Data: [Creative Commons Attribution 4.0 International](LICENSE). Attribute as
"European Semiconductor Atlas, richard-stahl.at".

`eu-grants.json` and `eu-grants-projects.json` are derived from CORDIS open
data, © European Union, CC BY 4.0.

## Corrections

If you find an error, or you operate one of these sites and have better numbers,
open an issue or write to kontakt@richard-stahl.at. Corrections are applied
with attribution, and a figure confirmed by the operator replaces any secondary
source.
