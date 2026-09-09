# Automated Loyverse Sales ETL Pipeline

> **Confidentiality Notice:** This project was developed for a private client. Production source code, sales data, credentials, and other client-specific information are intentionally excluded from this repository. This repository is provided for portfolio and technical demonstration purposes only.

## Why it was built

This pipeline was built to:

* Maintain **unlimited historical sales data** beyond Loyverse's limited retention period.
* **Remove repetitive manual data entry** by automatically collecting and storing sales records.
* Create an organized data foundation that provides **more opportunities for analysis** by combining sales, weather, and calendar data.

## What it does

```text
Loyverse POS
     │
     ▼
  Extract
     │
     ▼
 Transform
     │
     ├── Sales data
     ├── Weather data
     └── Calendar data
     │
     ▼
 PostgreSQL
     │
     ├── sales_data
     │
     └── daily_sales_summary
```

The pipeline automatically collects sales data, processes it, enriches it with weather and calendar information, and stores it in PostgreSQL for long-term use.

## Database Design

```sql
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

`sales_data` stores item-level sales records, while `daily_sales_summary` stores one persistent record per sales date.

The tables are conceptually related through `sales_date`; no foreign key is enforced between them.

## Automation

```text
GitHub Actions
      │
      │ scheduled run
      ▼
   Python ETL
      │
      ▼
 PostgreSQL
```

The pipeline runs automatically every night.

If no sales are available:

```text
No sales rows
     │
     ├── No itemized records inserted
     │
     └── Daily summary still recorded
```

Daily summaries use an `ON CONFLICT` upsert on `sales_date`, allowing existing dates to be updated when the pipeline runs again.

## Tech Stack

```text
Python
Playwright
PostgreSQL
Neon
REST APIs
Git
GitHub Actions
```

Production client data, credentials, and client-specific implementation details are kept private and are not included in this repository.
