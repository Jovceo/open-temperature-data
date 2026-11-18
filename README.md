# Open temperature reference datasets

Machine-readable temperature tables with row-level provenance: UK gas mark and oven conversions,
USDA safe minimum internal cooking temperatures, and human body temperature thresholds.

This repository is a **published snapshot** of the datasets served at
<https://ctofconverter.com/data-api>. It exists so that the data can be cloned, forked, cited and archived
without depending on a website staying up.

- **3 datasets, 55 rows.**
- **Every row states where it came from**: publishing body, URL, a verbatim fragment of the source
  text, and the date it was checked.
- **Disagreement is preserved, not averaged.** When two authorities publish different figures, both
  are kept and labelled. Bands that no authority publishes are deliberately absent.
- **Formats**: JSON (with column metadata, units and provenance) and RFC 4180 CSV, UTF-8 without BOM.
- **License**: data CC-BY-4.0, code MIT.

## Datasets

| Dataset | Rows | Version | Last checked | Formats |
| --- | --- | --- | --- | --- |
| [`human-body-temperature`](#human-body-temperature) | 34 | 1.0.0 | 2026-08-27 | JSON + CSV |
| [`oven-conversions`](#oven-conversions) | 12 | 1.0.0 | 2026-08-27 | JSON + CSV |
| [`safe-internal-temperatures`](#safe-internal-temperatures) | 9 | 1.0.0 | 2026-08-27 | JSON + CSV |

## Quick start

```bash
git clone https://github.com/Jovceo/open-temperature-data.git
cd open-temperature-data
```

Read the files directly, or use the read-only API (no key, no signup, CORS enabled):

```bash
curl https://ctofconverter.com/data/manifest.json
curl https://ctofconverter.com/data/oven-conversions.json
curl "https://ctofconverter.com/api/ref/oven-conversions?select=gas_mark,conventional_c,fan_c"
curl "https://ctofconverter.com/api/ref/lookup?value=180&unit=c&context=oven"
```

Embed a table on a page (dependency-free, no tracking):

```html
<script src="https://ctofconverter.com/embed/ref-chart.js" data-set="oven-conversions"></script>
```

Docs for the API and the embed widget: <https://ctofconverter.com/data-api>

## Data dictionary

### `human-body-temperature`

Every row is one number that a named public-health or clinical source actually publishes, with the URL it was read from and a verbatim fragment. Deliberately excludes any band that no source publishes.

- Rows: **34** · Version: **1.0.0** · Last checked: **2026-08-27** · License: **CC-BY-4.0**
- Files: [`human-body-temperature.json`](./data/human-body-temperature.json) · [`human-body-temperature.csv`](./data/human-body-temperature.csv)

| Column | Meaning | Unit | Type |
| --- | --- | --- | --- |
| `id` | id |  | string |
| `topic` | Topic |  | string |
| `applies_to` | Applies to |  | string |
| `audience` | Audience |  | string |
| `site` | Measurement site |  | string |
| `setting` | Setting |  | string |
| `rule` | Rule |  | string |
| `c_min` | From | °C | number |
| `c_max` | To | °C | number |
| `f_min` | From | °F | number |
| `f_max` | To | °F | number |
| `offset_c` | Offset | °C | number |
| `duration_days` | Duration | days | number |
| `label` | Label |  | string |
| `action` | What it means / what to do |  | string |
| `source` | Source |  | string |
| `source_url` | Source URL |  | string |
| `quote` | Verbatim fragment |  | string |
| `note` | Note |  | string |
| `checked_on` | Checked on |  | string |

### `oven-conversions`

Gas mark ↔ conventional °C/°F ↔ fan-assisted °C/°F, with the published fan reduction convention stated separately from the measured values.

- Rows: **12** · Version: **1.0.0** · Last checked: **2026-08-27** · License: **CC-BY-4.0**
- Files: [`oven-conversions.json`](./data/oven-conversions.json) · [`oven-conversions.csv`](./data/oven-conversions.csv)

| Column | Meaning | Unit | Type |
| --- | --- | --- | --- |
| `gas_mark` | UK Gas Mark |  | string |
| `conventional_c` | Conventional oven | °C | number |
| `conventional_f` | Conventional oven | °F | number |
| `fan_c` | Fan / convection oven | °C | number |
| `fan_f` | Fan / convection oven | °F | number |
| `descriptor` | Heat level |  | string |
| `typical_uses` | Typical uses |  | string |
| `source` | Source |  | string |
| `source_url` | Source URL |  | string |
| `quote` | Verbatim source fragment |  | string |
| `checked_on` | Checked on |  | string |

### `safe-internal-temperatures`

Minimum internal temperatures by product, with rest times, as published by USDA FSIS, plus the danger zone.

- Rows: **9** · Version: **1.0.0** · Last checked: **2026-08-27** · License: **CC-BY-4.0**
- Files: [`safe-internal-temperatures.json`](./data/safe-internal-temperatures.json) · [`safe-internal-temperatures.csv`](./data/safe-internal-temperatures.csv)

| Column | Meaning | Unit | Type |
| --- | --- | --- | --- |
| `product` | Product |  | string |
| `min_fahrenheit` | Minimum internal temperature | °F | number |
| `min_celsius` | Minimum internal temperature | °C | number |
| `rest_minutes` | Rest time | min | number |
| `note` | Note |  | string |
| `source` | Source |  | string |
| `source_url` | Source URL |  | string |
| `quote` | Verbatim source fragment |  | string |
| `checked_on` | Checked on |  | string |

## Provenance

Each dataset carries a `provenance` array (who was read, at which URL, when, and what was
noted), and each row carries `source`, `source_url`, `quote` and `checked_on`. The site's CI
refuses to publish a dataset whose rows lack any of those fields, which is why you can audit any
number here without trusting this project.

<details>
<summary><code>human-body-temperature</code> — 8 source(s)</summary>

- **NHS — Fever in adults / Fever in children / Hypothermia / How to take your baby's temperature** — <https://www.nhs.uk/symptoms/fever-in-adults/> · checked 2026-08-27
  - All four NHS pages re-fetched from this machine on 2026-08-27 and returned HTTP 200 with the quoted strings present. Other NHS URLs used: /symptoms/fever-in-children/, /conditions/hypothermia/, /baby/health/how-to-take-your-babys-temperature/.
- **StatPearls (NCBI Bookshelf) — Hypothermia (NBK545239) and Physiology, Fever (NBK562334)** — <https://www.ncbi.nlm.nih.gov/books/NBK545239/> · checked 2026-08-27
  - Both chapters re-fetched 2026-08-27 (HTTP 200). NBK545239 is the source for the 35 °C hypothermia definition, the 36.5–37.5 °C normal range and the mild/moderate/severe staging. NBK562334 is the source for the low/moderate/high/hyperthermia fever bands and the per-site average readings.
- **Merck Manual, Professional Version — Fever** — <https://www.merckmanuals.com/professional/infectious-diseases/biology-of-infectious-disease/fever> · checked 2026-08-27
  - Re-fetched 2026-08-27 (HTTP 200). Source for the 40 °C / 35 °C red flags and the 0.6 °C site offsets.
- **NICE CG65 — Hypothermia in the perioperative period (via NCBI Bookshelf NBK554181)** — <https://www.ncbi.nlm.nih.gov/books/NBK554181/> · checked 2026-08-27
  - Re-fetched 2026-08-27 (HTTP 200). Source for the separate perioperative 'low core temperature' cut-off of 36.0 °C, which is NOT a definition of hypothermia.
- **Royal Children's Hospital Melbourne — Temperature management (paediatric guideline)** — <https://www.rch.org.au/rchcpg/hospital_clinical_guideline_index/Temperature_management/> · checked 2026-08-27
  - Re-fetched 2026-08-27 (HTTP 200). Source for the paediatric / neonatal low-temperature thresholds used in hospital care.
- **Mayo Clinic and CDC pages** — <https://www.mayoclinic.org/first-aid/first-aid-fever/basics/art-20056685> · checked 2026-08-27
  - Quotes were captured by a manual read of these pages in a browser session on 2026-08-27. Automated re-fetch of mayoclinic.org and cdc.gov from this build machine returns HTTP 403 (bot protection), so the numbers could not be re-verified by script — they are kept here only where they agree with a machine-verified source above.
- **PMC6456186 — systematic review of normal body temperature values (9,227 measurements)** — <https://pmc.ncbi.nlm.nih.gov/articles/PMC6456186/> · checked 2026-08-27
  - Re-fetched 2026-08-27 (HTTP 200). Used to cross-check the per-site averages published in StatPearls NBK562334; the two agree.
- **Field notes on scope** — <https://ctofconverter.com/data-api> · checked 2026-08-27
  - Rows carry a "setting" value (home | hospital | reference | device) and an "audience" value (any | adult | child | infant | neonate). The lookup endpoint applies only home rows unless you pass ?setting=all, and ?person= narrows by age group, so a home reader is never shown an inpatient escalation threshold or another age group's cut-off.

</details>

<details>
<summary><code>oven-conversions</code> — 2 source(s)</summary>

- **Wikipedia — Gas mark, 'Conversion table' (its own references: BBC Good Food 'Conversion Guides'; The Guardian, 'Cooking conversion charts', 24 Nov 2007)** — <https://en.wikipedia.org/wiki/Gas_mark> · checked 2026-08-27
  - Secondary source, used because it reproduces the two published conversion guides in one table. Conventional-oven values in this dataset are taken verbatim from that table (110/120/140/150/170/180/190/200/220/230/240/260 °C at 25 °F steps).
- **ctofconverter.com — Fan oven conversion chart** — <https://ctofconverter.com/fan-oven-conversion-chart> · checked 2026-08-27
  - The site's own gas-mark rows were corrected to match the table above on 2026-08-27. The previous revision labelled conventional values as fan values and had Gas Mark 3, 5 and 7 wrong; do not copy the pre-2026-08-27 revision.

</details>

<details>
<summary><code>safe-internal-temperatures</code> — 2 source(s)</summary>

- **USDA FSIS — Safe Minimum Internal Temperature Chart** — <https://www.fsis.usda.gov/food-safety/safe-food-handling-and-preparation/food-safety-basics/safe-temperature-chart> · checked 2026-08-27
  - Page states 'Last Updated: Apr 14, 2025'. Every row below reproduces that table verbatim, including the exact Celsius values FSIS prints (62.8 / 71.1 / 73.9 / 60 °C), which are the Fahrenheit figures converted, not independently rounded.
- **USDA FSIS — Danger Zone 40F - 140F** — <https://www.fsis.usda.gov/food-safety/safe-food-handling-and-preparation/food-safety-basics/danger-zone-40f-140f> · checked 2026-08-27
  - The 40–140 °F range is taken from the section title as listed in FSIS navigation on 2026-08-27; the page body could not be re-fetched here because fsis.usda.gov returns 403 to non-browser clients. Treat the range as stable (it is long-standing USDA guidance) and re-check the page itself before republishing at scale.

</details>

## How this repository stays in sync

The single source of truth is the `config/datasets/` directory of the
[ctofconverter](https://ctofconverter.com) repository. The site's build generates
<https://ctofconverter.com/data/*>, and this repository mirrors that published layer:

1. `scripts/sync-from-site.mjs` downloads `manifest.json` and every dataset file from
   <https://ctofconverter.com/data/>.
2. It validates structure and provenance, compares the bytes with the files in `data/`, and
   rewrites them only if the published layer actually changed.
3. `MANIFEST.sha256` is regenerated, and `SYNC-STATE.json` records when the check last ran and
   what the result was. A scheduled GitHub Actions workflow (`.github/workflows/sync.yml`, weekly
   plus manual dispatch) commits that record on every run, and the dataset files themselves only
   when the site's published bytes actually changed.

A manual run is safe and idempotent:

```bash
node scripts/sync-from-site.mjs           # pull + validate + write
node scripts/sync-from-site.mjs --check   # verify only, non-zero exit if out of sync
```

Because drift is corrected by CI rather than by memory, a snapshot here cannot quietly diverge
from what the site serves. `SYNC-STATE.json` is the freshness signal: if `lastChecked` is more than
a few weeks old, the workflow itself has stopped — open the Actions tab and use **Run workflow**.
(GitHub disables scheduled workflows on long-inactive repositories; the weekly state commit exists
to prevent exactly that.)

## Versioning

Each dataset has its own `version` and `updated` date inside its file; a correction bumps both and
never rewrites history. Repository snapshots are labelled `YYYY.MM.DD` (`2026.11.18` for the initial
import), and any change lands as a commit with the diff visible.

## Citation

See [`CITATION.cff`](./CITATION.cff). A DOI will be minted from a Zenodo release; until one
exists, cite the repository and the snapshot version:

```
Open temperature reference datasets (2026.11.18). https://github.com/Jovceo/open-temperature-data
```

Always cite the authorities named in each dataset's provenance block as well. These files are a
curation layer over their work, not a replacement for it.

## License

- **Data** (`data/*.json`, `data/*.csv`, `data/manifest.json`): CC-BY-4.0 — see
  [`LICENSE-DATA.md`](./LICENSE-DATA.md).
- **Code** (`scripts/*`, `.github/workflows/*`): MIT — see [`LICENSE`](./LICENSE).

Attribution for the data is one line:

```
Temperature data: ctofconverter.com — CC-BY-4.0
https://ctofconverter.com/data-api
```

The authorities cited in each provenance block are not relicensed here. If you redistribute a
table, cite them as well.

## Scope and limits

- This is not a source of authority. The authorities are the ones named in each provenance block;
  read them there and cite them there.
- The body-temperature dataset is a reference table of published thresholds, **not medical
  advice**, and must not be the only basis for a decision about a sick person.
- Fan-oven figures are a convention (commonly 20 °C below conventional), not a measurement.
  Ovens vary; use an oven thermometer for anything temperature-sensitive.

## Corrections

If a number here does not match its cited source, that is a bug. Open an issue with the dataset id
and the row, or write to the address in the site's footer. Corrections ship as a new dataset
version, with the provenance block updated.
