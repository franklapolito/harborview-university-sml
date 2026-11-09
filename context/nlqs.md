# Hero questions, with answers certified against the warehouse

Computed in SQL against `HARBORVIEW_UNIVERSITY_DEMO.UNIVERSITY`
(`snowflake/nlq_smoke_tests.sql`). The model must return the same numbers from
Claude Desktop, Power BI and Excel. Data is as of 31 October 2026: the Fall
2026 census is in.

| # | Question (as a seller would ask it) | Certified answer | What it proves |
|---|---|---|---|
| 1 | How many students are enrolled this fall, and how many FTE? | Fall 2026 census: 38,776 students (32,602 undergraduate, 6,174 graduate), 35,379.8 FTE, 516,929.5 credit hours, $392.9M tuition. | The baseline. Same number in every tool. |
| 2 | How many students did we have in 2026? | 38,776, the Fall 2026 census of record. A raw-SQL agent sums Spring, Summer and Fall and answers 85,687. | The snapshot trap: headcount is a census per term, never a sum. |
| 3 | What is our fall-to-fall retention rate for the Fall 2025 cohort? | 86.7%: 6,902 of the 7,960 first-time full-time cohort returned. Engineering 89.3% to Education 81.5%. A raw-SQL agent dividing by all 9,699 new first-years answers 71.2%. | The cohort trap: the IPEDS denominator lives in the model. |
| 4 | How much of award HSU-1042's Year 2 budget has been spent? | 78.0%: $1,092,000 of $1,400,000 for the period 1 Sep 2025 to 31 Aug 2026. A raw-SQL agent using FY2026 answers $1,136,573, a different window. | The award-year trap: sponsors count on their calendar, not the university's. |
| 5 | Which colleges are growing and which are shrinking, fall over fall? | Computing +8.7%, Engineering +4.3%, Business +2.8%, Nursing +2.8%, Public Policy +0.1%; Arts & Sciences −1.4%, Fine Arts −3.0%, Education −4.5%. | Prior-year time intelligence on the academic calendar. |
| 6 | What is the six-year graduation rate for the Fall 2020 cohort? | 63.3% (4,459 of 7,048). In-State 64.1%, International 62.5%, Out-of-State 60.6%. | Only cohorts old enough to observe count in the denominator. |
| 7 | How much sponsored research did we spend in FY2026, and from whom? | $48.7M: Federal $43.7M (90%), State $2.1M, Foundation $1.7M, Industry $1.2M. Engineering $15.5M, Nursing $9.4M, Computing $8.7M. | Fiscal-year view of grants, next to the award-period view. |
| 8 | How is international enrollment doing, and what does it mean for tuition? | 4,490 in Fall 2026, down 5.0% from 4,727; international tuition $83.0M, down from $85.2M. International pays $1,465 per credit hour against $496 in-state. | A chain across headcount, revenue and rate. |
| 9 | Which programmes are growing fastest? | Health Information Management +15.5%, MS Computer Science +12.6%, Data Science +11.1%, Electrical Engineering +10.8%, Biomedical Engineering +10.4%. | Program-level growth, same measure. |
| 10 | Fall 2026 headcount by campus (the Power BI and Excel table) | Main 26,318; Harbor Downtown 8,982; Northshore 3,476. | The BI triangle. |

## The three raw-SQL contrast questions

Questions 2, 3 and 4. Each raw answer is wrong in a way the governed
definition prevents, and the wrong numbers are known in advance: 85,687,
71.2% and $1,136,573.

## One rule for the agent

Census Headcount and Census FTE are snapshots. A question about them must
carry a Term, Academic Year or Fiscal Year; with no time level the engine
sums every census since 2022. The metric descriptions say so and the MCP
server warns when it is about to happen.
