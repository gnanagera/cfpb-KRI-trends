# Complaint Risk Reporting: KRI Scorecard

A risk-reporting project built on the public CFPB Consumer Complaint Database. It shows the core workflow of a risk analytics role: load raw data, clean and validate it, define a Key Risk Indicator (KRI) with thresholds, and report it in a dashboard for decision-makers.

## Business question

Are companies responding to consumer complaints on time, and in which months did the rate of untimely responses breach an acceptable risk level?

## Tools

- **SQL Server (T-SQL):** staging, cleaning, and KRI logic
- **Power BI Desktop:** scorecard and trend visuals

## Data

- **Source:** [CFPB Consumer Complaint Database](https://www.consumerfinance.gov/data-research/consumer-complaints/)
- **Period covered:** October 2023 to September 2026 (complete months only)
- **Downloaded on:** [add date]

The raw CSV is not included in this repository because of its size. To reproduce the project, download it from the link above.

## Approach

1. **Staging.** Loaded the raw CSV unchanged into `stg_complaints`, kept as the source of record.
2. **Cleaning.** Built `complaints_clean` with typed dates, trimmed text, standardized state codes, and a numeric `timely_flag` (1 = timely, 0 = untimely).
3. **KRI logic.** Built the view `vw_kri_simple`, which calculates monthly complaint volume, the percentage of untimely responses, and a Red/Amber/Green status.
4. **Reporting.** Loaded the view into Power BI and built a scorecard table and a trend chart showing the red threshold.

## KRI definition

| KRI | Calculation | Green | Amber | Red |
|---|---|---|---|---|
| % untimely responses | untimely complaints / total complaints received in the month | below 2% | 2% to 5% | above 5% |

The thresholds are illustrative assumptions. In a real organization, risk committees set and approve them.

## Key findings

- Untimely responses were low through most of 2024, mostly under 1% from April to December.
- **March to May 2025** was red for three consecutive months, peaking at **7.9% in April 2025**.
- The KRI turned red again in **January 2026** and **March to June 2026**. June 2026 was 7.08%.
- Monthly complaint volume rose over the period, from about 400 to 700 in late 2023 and early 2024 to roughly 1,000 to 1,300 in 2026.
- September 2026 shows 0.64%, which may be understated because recent complaints have had less time to become late.

## Data quality notes and limitations

- The current, incomplete month is excluded. A partial month understates the untimely rate.
- The most recent complete month may be understated for the same reason.
- This is public data with no internal account counts, so volume-based measures such as complaints per 10,000 accounts cannot be calculated.
- Thresholds are assumptions, not regulatory standards.
- The findings describe patterns in the data. They do not explain the causes.

## Repository structure

```
sql/        T-SQL scripts, numbered in run order
powerbi/    Power BI report file (.pbix)
docs/       Dashboard screenshots
README.md
```

## How to reproduce

1. Download the CFPB complaints CSV and load it into a table named `stg_complaints`.
2. Run the scripts in `sql/` in numeric order.
3. Open `powerbi/risk_scorecard.pbix` and point the data source at your own SQL Server instance.

## Possible extensions

- Data quality exception rules with an issue log
- Control-testing sample and workpaper
- Additional KRIs such as relief rate and product concentration

## Author

[Your name] | [LinkedIn URL]
