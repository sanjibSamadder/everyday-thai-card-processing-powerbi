![Dashboard](images/dashboard.png)

<div align="center">

# Card Processing Performance Dashboard

### A Monthly Power BI Report on Sales, Transactions, Fees and Effective Rate

![Power BI](https://img.shields.io/badge/Power%20BI-Dashboard-F2C811?logo=powerbi&logoColor=black)
![DAX](https://img.shields.io/badge/DAX-MTD%20%7C%20YTD%20%7C%20Prior%20year-E66C37)
![Period](https://img.shields.io/badge/Period-Oct%202021%20to%20Apr%202024-0F172A)
![Model](https://img.shields.io/badge/Model-Star%20schema-0E9F9A)

**One page shows what was processed, what it cost, and which card types cost the most**

</div>

---

## Overview

This project turns monthly merchant-statement data from a restaurant (Everyday Thai)
into a one-page Power BI report for the April 2024 reporting month.

Sales totals alone do not show what card payments really cost. The owner needs to know
how this year compares with last year, whether transaction volume is holding up, what
share of each sale goes to processing fees, and which card types push that cost up.
The report answers these with year-over-year comparisons, trend lines and a card-level
breakdown table.

![Data model](images/data_model.png)

## Key Result

> In April 2024 the business processed **$173.61K across 3,664 transactions** at an
> **effective rate of 2.41%**. Year to date, sales are up **0.49%** on last year while
> fees are up **2.17%**, so fees are growing faster than sales.

| April 2024 (month to date) | Value |
|---|---|
| Amount processed | $173.61K |
| Transactions | 3,664 |
| Effective rate | 2.41% |
| Average sale per transaction | $47.38 |
| Average fee per transaction | $1.14 |

| Year to date (Jan to Apr 2024) | 2024 | Last year | Change |
|---|---|---|---|
| Amount | $782.03K | $778.19K | +0.49% |
| Transactions | 17.07K | 17.18K | -0.65% |
| Fees | $18.85K | $18.45K | +2.17% |

| Full-year view | Amount | Transactions |
|---|---|---|
| 2021 | $2.24M | 55K |
| 2022 | $2.38M | 53K |
| 2023 | $2.32M | 51K |
| Expected 2024 | $2.35M | 51.22K |

The expected 2024 figures come from the `ExpectedAmny24` and `ExpQTN24` measures.

## Main Findings

1. **Fees are growing faster than sales.** Year-to-date amount is up 0.49% and
   transactions are down 0.65%, yet fees are up 2.17%.
2. **Keyed-in cards are the most expensive.** V/MC/D keyed transactions cost 3.69%
   against 2.28% when swiped. Amex keyed costs 4.18% against 3.56% swiped.
3. **Keyed volume is small but costly.** Keyed cards are about 6% of all volume
   processed but about 9% of all fees collected.
4. **Amex costs more than V/MC/D across the board.** Amex sits at 3.56% to 4.18%,
   while V/MC/D sits at 2.28% to 3.69%.
5. **The effective rate is stable month to month.** In 2024 it moved only between
   2.40% and 2.42%, so the cost is driven by card mix, not by monthly swings.
6. **Transaction counts are drifting down while average sale is steady.** Yearly
   volume went from 55K to 53K to 51K, so sales are held up by ticket size.

**Back-of-envelope saving:** if V/MC/D keyed volume ($451,983 in the data) had been
charged at the standard V/MC/D rate of 2.34%, fees would have been about $6,100 lower.
This is illustrative only and assumes keyed sales could have been swiped.

## Dashboard Components

| # | Section | What it shows | Key measures |
|---|---|---|---|
| 1 | **KPI cards** | Current amount, count, effective rate, average sale, average fee | `MTD_Amount`, `Effective Rate` |
| 2 | **Amount** | YTD vs last year, 2021 to 2023 totals, expected 2024, monthly columns by year | `ExpectedAmny24` |
| 3 | **Quantity** | YTD vs last year, yearly totals, expected 2024, monthly stepped area chart | `ExpQTN24` |
| 4 | **Fee** | YTD fees vs last year, monthly line chart with trend line | Fees from `T1` |
| 5 | **Effective Rate** | Monthly rate for 2024 with trend line | `Effective Rate` |
| 6 | **Card table** | Payments, count, fees, refunds and rate by card type | `Card Processing` |

**Card types tracked:** V/MC/D, V/MC/D Swipe, V/MC/D (Keyed), Amex, Amex Swipe, Amex (Keyed).

## Data Model

A star-style model with one shared date table.

```
            Sheet1 (Date)
             1 /      \ 1
              /        \
             *          *
     Card Processing     T1
```

| Table | Role | Contents |
|---|---|---|
| **Card Processing** | Fact table, 159 rows | Statement date, card type, rate, payments (# and $), refunds (# and $), fees collected, fee adjustments, net |
| **T1** | Monthly summary fact table | Date, month, quarter, year, payments, refunds, fees, net deposit |
| **Sheet1** | Date dimension | `Date`, related one-to-many to both fact tables |
| **Measure** | Measure table | `Effective Rate`, `MTD_Amount`, `ExpectedAmny24`, `ExpQTN24` and others |

**Metric definitions**

| Metric | Definition |
|---|---|
| Effective rate | Fees collected divided by payments ($) |
| Average sale per transaction | Payments ($) divided by payments (#) |
| Average fee per transaction | Fees collected divided by payments (#) |

**Source rate structure** (from the statement data)

| Card type | Rate |
|---|---|
| V/MC/D | 1.95% + $0.15 |
| V/MC/D (Keyed) | 3.2% + $0.15 |
| Amex | 3.29% + $0.15 |

## Power BI Skills Shown

| Skill | Where it is used |
|---|---|
| Star-schema data modelling | Date table linked to two fact tables |
| DAX time intelligence | MTD, YTD and last-year comparisons |
| Projection measures | Expected 2024 amount and quantity |
| KPI cards with variance indicators | Header cards and YTD blocks |
| Trend lines and conditional highlighting | Fee and rate charts, 2024 bars highlighted |
| Data cleaning in Power Query | Card type labels (`Card Type` to `Card Type 2`) |
| Financial analysis | Effective rate and fee-mix analysis |

## Dataset

| Property | Value |
|---|---|
| Card-level records | 159 rows (statement month and card type) |
| Card-level period | October 2021 to April 2024 |
| Monthly summary period | January 2021 onward (`T1`) |
| Source | Monthly card-processing merchant statements |

## Repository Structure

```
.
├── Everyday Thai Till April 2024 Updated.pbix   # Power BI report
├── Dashboard.pdf                                # Exported dashboard
├── images/
│   ├── dashboard.png                            # Screenshot used in this README
│   └── data_model.png                           # Data model screenshot
└── README.md
```

## Getting Started

**1. Clone the repository**

```bash
git clone https://github.com/sanjibSamadder/everyday-thai-card-processing-powerbi.git
cd everyday-thai-card-processing-powerbi
```

**2. Open the report** in [Power BI Desktop](https://powerbi.microsoft.com/desktop/)
(free, Windows): open `Everyday Thai Till April 2024 Updated.pbix`.

**3. Point to your data.** If the source path has changed, go to
**Home > Transform data > Data source settings** and update it.

**4. Update for a new month** by adding the new statement rows to the source and
clicking **Refresh**. A static copy of the report is in `Dashboard.pdf`.


## Provenance & License

**Source:** monthly card-processing statements from a restaurant business, taken from
the author's work.


## Author

**Sanjib Samadder**

**📬 Let's connect!** I'm open to discussions about data analytics, dashboards, and collaborative projects.

[![Email](https://img.shields.io/badge/Email-skilled.sanjib%40gmail.com-red?logo=gmail)](mailto:skilled.sanjib@gmail.com)
[![GitHub](https://img.shields.io/badge/GitHub-sanjibSamadder-181717?logo=github)](https://github.com/sanjibSamadder)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-Sanjib%20Samadder-0A66C2?logo=linkedin)](https://www.linkedin.com/in/sanjibsamadder/)

**Happy Dashboarding!** 📊

## Disclaimer

This project is for educational and portfolio purposes only. Figures are taken from
merchant statements and are not financial advice.
