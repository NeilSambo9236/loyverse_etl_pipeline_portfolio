# Automated Loyverse Sales ETL Pipeline

> **Confidentiality & Usage Notice:** This project was developed for a private client. The production implementation and source code are intentionally not published due to client confidentiality and project ownership considerations. The client has not authorized the release of the production code, data, credentials, or configuration. This repository therefore provides **project evidence, architecture, database design, and technical documentation** to demonstrate the work without exposing the underlying implementation.

## Why it was built

This pipeline was built to:

* Preserve historical sales data beyond Loyverse's limited retention period.
* Automate the collection and processing of sales records, reducing repetitive manual data entry.
* Combine sales data with weather and calendar information to provide a foundation for analysis.

## What it does

```text
Loyverse POS
     │
     ▼
Loyverse API
     │
     ▼
  Extract
     │
     ├── Sales
     ├── Refunds
     └── Reference data
     │
     ▼
 Transform
     │
     ├── Sales metrics
     ├── Weather
     └── Calendar
     │
     ▼
 PostgreSQL
     │
     ├── sales_data
     └── daily_sales_summary
```

The pipeline retrieves paginated sales and refund data from the **Loyverse API**, transforms the records, enriches daily summaries with weather and calendar data, and stores the results in PostgreSQL.

## Database Design

```text
sales_data
──────────
id
sales_date
item_name
category
items_sold
gross_sales
items_refunded
refunds
discounts
net_sales
cost_of_goods
gross_profit
margin
created_at


daily_sales_summary
───────────────────
sales_date PK
gross_profit
cost_of_goods
net_sales
precipitation_sum_mm
precipitation_hours
precipitation_probability_max_percent
weather_code
weather_code_description
maximum_temperature_c
holiday
month_name
days_of_week
updated_at
```

`sales_data` contains item-level records, while `daily_sales_summary` contains one record per sales date. The tables are related conceptually through `sales_date`.

## Automation

```text
GitHub Actions
      │
      │ 9:00 AM PHT
      ▼
 Previous Day's Data
      │
      ▼
   Python ETL
      │
      ▼
 PostgreSQL
```

The pipeline runs automatically every day at **9:00 AM Philippine time** and processes the **previous Philippine calendar day's data**.

The schedule provides a buffer for data availability and helps reduce the impact of GitHub Actions scheduling delays or temporary technical issues. The runner uses the `Asia/Manila` timezone to keep date processing aligned with the business.

The workflow can also be triggered manually for testing or reruns.

If no sales are available, no itemized records are inserted, but the daily summary is still recorded. Daily summaries use an `ON CONFLICT` upsert on `sales_date` so existing dates can be updated when rerun.

## Tech Stack

```text
Python
Pandas
Loyverse API
WeatherAPI
PostgreSQL
Neon
Git
GitHub Actions
```

### Repository Contents

The repository contains **evidence of the project, architecture, database design, and technical documentation** rather than the production source code.

Production client data, credentials, configuration, and implementation details remain private and are not included in this repository.
