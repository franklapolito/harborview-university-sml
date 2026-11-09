# Harborview University, AtScale SML model

Enrollment, retention and sponsored research for a fictional three-campus
flagship university: one governed definition of census headcount, FTE, credit
hours, tuition, the first-time full-time cohort, retention and graduation
rates, and grant burn by award period, served to Power BI, Excel and every MCP
client. Built for the FRC SLED seller enablement (use case 3 of the SLED
briefing: higher-education enrollment, retention and research-grant analytics).

Emitted by `harborview-university/sml/emit_sml.py`; data by
`harborview-university/data/generate.py` (seed 42); loaded by `snowflake/load.sh`.

## Inputs

| File in `context/` | What it is |
|---|---|
| `use_case.md` | Governed definitions, grain, the engineered stories with numbers, limitations. |
| `nlqs.md` | The hero questions with answers certified against the warehouse. |
| `ddl.sql` | Snowflake DDL for the ten tables, with table comments. |
| `erd.mmd` | Mermaid ERD: three facts, six dimension tables in the model. |
| `build.yaml` | The build parameters this emit used. |
| Data profile | (not provided; the warehouse was verified by `04_validate.sql`) |
| Existing SML files | (not provided; new model) |

Target warehouse: **Snowflake** (`HARBORVIEW_UNIVERSITY_DEMO.UNIVERSITY`).

## Build parameters

| Parameter | Value | Source |
|---|---|---|
| `unrelated_dimension_handling` | `empty` | explicit |
| `warehouse` | Snowflake | explicit |
| `as_connection` | `SnowflakePartner` | explicit (`SnowflakePartner` on demo2; `con1` on eshack and swtdev) |
| `database` / `schema` | `HARBORVIEW_UNIVERSITY_DEMO` / `UNIVERSITY` | explicit |
| `model_unique_name` | `harborview_university` | explicit |
| `catalog_unique_name` | `harborview_university_catalog` | explicit |
| `currency` | USD | explicit |
| `time_window` | academic (Fall-Spring-Summer), fiscal (Jul-Jun) and calendar hierarchies on one dimension | explicit |
| `use_cases_covered` | census enrollment, retention and graduation cohorts, sponsored research spend | explicit |
| `use_cases_excluded` | admissions funnel, financial aid, course sections, payroll | explicit |
| `semi_additive_default` | `first` on Census Headcount and Census FTE over the enrollment fact's Date relationship | explicit |
| SML catalog `version` | `1.9` | explicit; demo2 accepted 1.9 |

## Assumptions and decisions

- **Three hierarchies in one Date Dimension share the `Day` leaf**; every upper level has
  a distinct unique name. Term, Fiscal Quarter, Fiscal Month, Quarter and Month each
  carry a prior-year key at their own grain; the top levels declare none; Day declares
  none because the facts are term-grain or month-grain.
- **Census Headcount and Census FTE are `semi_additive: first`** over the enrollment
  fact's Date relationship, so an academic year or fiscal year returns the fall census
  (its first term) and a calendar year returns Spring, which the descriptions call out.
  Credit Hours and Tuition Revenue are plain sums.
- **Retention and graduation rates are ratios of additive counts**, with the observable
  cohort size as the graduation denominator so cohorts too young to graduate do not
  dilute the rate. All New First-Year Students is published so the wrong denominator
  can be shown, with its description saying so.
- **The retention and grant facts join the Program Dimension at the College level** by
  college name; by Program they are empty. The grant fact also joins the Sponsored
  Award Dimension at its Award Period leaf; that dimension is one flat table carrying
  sponsor, award and period.
- **Award Period Budget rides on the period's first month row**, so a plain sum is the
  period budget at any grouping that contains the whole period.
- **`unrelated_dimensions_handling: empty` on every base metric.**
- **No real university, agency, sponsor, programme or person.** Sponsor names are
  generic.

## Generation summary

| Object | Count |
|---|---|
| Datasets | 9 (3 facts, 6 dimension sources) |
| Dimensions | 6 |
| Hierarchies | 8 |
| Level attributes | 19 |
| Secondary attributes | 16 |
| Base metrics | 16 |
| Calculated metrics | 14 |
| Model relationships | 12 |
| Calculated columns | 2 (`TOTAL_COST` on the grant fact, `AWARD_LABEL` on the award period dimension) |
| Role-play prefixes | none |
| Snowflake bridges | none |

Metric folders: Enrollment, Retention, Sponsored Research, Time Relative.

## Reproducing this build

`context/` holds verbatim copies of every input. Re-running the emitter with the same
parameters produces identical YAML; `data/generate.py --seed 42` produces byte-identical CSVs.
