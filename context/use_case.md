# Harborview State University: the governed definitions

Harborview State University is a fictional three-campus flagship. Institutional
research, the provost's office, finance and sponsored programs all report
enrolment, retention and grant spend, in two or three BI tools at once, each
with its own definitions. This document is the governed definition. The model
is the only place it is enforced.

## The three calendars

- **Academic:** Academic Year > Term > Day. Fall runs 16 August to 31 December,
  Spring 1 January to 15 May, Summer 16 May to 15 August. AY2026-27 is Fall
  2026, Spring 2027, Summer 2027. Census days are 15 September, 1 February and
  15 June. **The year's headcount is the fall census.**
- **Fiscal:** 1 July to 30 June, named for the year it ends in. FY2027 started
  1 July 2026.
- **Award periods:** each sponsored award runs in twelve-month budget years
  from its own start date. HSU-1042 Year 2 is 1 September 2025 to 31 August
  2026, which overlaps FY2026 and FY2027.

Data is as of **31 October 2026**: the Fall 2026 census is in, FY2027 is four
months old.

## Grain

| Table | Grain | Rows |
|---|---|---|
| `FACT_ENROLLMENT_CENSUS` | term census × campus × program × classification × residency | 12,636 |
| `FACT_RETENTION_COHORT` | entering fall cohort × campus × college × residency | 501 |
| `FACT_GRANT_ACTIVITY` | month-end × award period | 3,281 |
| `DIM_DATE` | day, 2018-07-01 to 2027-06-30 | 3,287 |
| `DIM_PROGRAM` | program (College > Program), 8 colleges | 57 |
| `DIM_AWARD_PERIOD` | award period (Sponsor Type > Sponsor > Award > Award Period) | 432 |
| `DIM_CAMPUS`, `DIM_CLASSIFICATION`, `DIM_RESIDENCY`, `DIM_AWARD` | 3, 6, 3, 120 | |

## Governed definitions

- **Census Headcount** is students enrolled on the term's census day. A
  snapshot per term, never summed across terms. For any year it is the fall
  census.
- **Census FTE** = credit hours ÷ 15 (undergraduate) or ÷ 12 (graduate), on the
  census day. Also a snapshot.
- **Credit Hours** and **Tuition Revenue** are additive across terms.
- **FTFT Cohort** is first-time, full-time, degree-seeking students entering in
  the fall: the IPEDS cohort and the only valid denominator. **All New
  First-Year Students** (adds part-time, non-degree, late admits) is published
  so the wrong denominator can be shown, and labelled as wrong.
- **Fall-to-Fall Retention Rate** = Retained to Second Fall ÷ FTFT Cohort.
- **Six-Year (Four-Year) Graduation Rate** = graduates ÷ the cohort size of
  cohorts old enough to observe it (Fall 2020 and earlier for six years, Fall
  2022 and earlier for four).
- **Grant Expenditures** = direct + indirect costs in the month. **Award Budget
  Allotted** is the period budget spread over its twelve months. **Grant Burn
  Rate** = expenditures ÷ allotted. **Award Period Budget Spent** =
  expenditures ÷ the full period budget.

## Engineered stories

| | Story | The number |
|---|---|---|
| S1 | **Headcount is a snapshot.** Summing 2026's three censuses more than doubles the university. | Fall 2026: **38,776**. Sum of Spring, Summer, Fall 2026: 85,687. FTE 35,379.8; credit hours 516,929.5; tuition $392.9M. |
| S2 | **The right cohort.** Retention on the FTFT cohort versus on all new first-years. | Fall 2025 cohort: **86.7%** (6,902 of 7,960). On 9,699 new first-years: 71.2%. |
| S3 | **Award year versus fiscal year.** | HSU-1042 Year 2 (Sep 2025 to Aug 2026): **$1,092,000 of $1,400,000, 78.0%**. The FY2026 window: $1,136,573. |
| S4 | **Growth by college, fall over fall.** | Computing +8.7%, Engineering +4.3%; Education −4.5%, Fine Arts −3.0%. |
| S5 | **International enrolment.** | 4,490, down 5.0% from 4,727; tuition $83.0M from $85.2M; $1,465 per credit hour versus $496 in-state. |
| S6 | **Six-year graduation, observable cohorts only.** | Fall 2020 cohort **63.3%** (4,459 of 7,048). |
| | **Sponsored research FY2026.** | $48.7M; Federal $43.7M; Engineering $15.5M. |

## Known limitations

- No admissions funnel, financial aid, course sections or payroll.
- Revenue is tuition only; no state appropriation or auxiliaries.
- The retention and grant facts carry college, not program.
- Graduation counts for cohorts too young to observe are zero, not null, with
  a zero denominator, so rates are empty rather than wrong.
