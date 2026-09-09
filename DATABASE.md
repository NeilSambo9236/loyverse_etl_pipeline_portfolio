# Database Design

```sql
CREATE TABLE sales_data (
    id SERIAL PRIMARY KEY,
    sales_date DATE NOT NULL,
    item_name VARCHAR(255),
    category VARCHAR(255),
    items_sold NUMERIC,
    gross_sales NUMERIC,
    items_refunded NUMERIC,
    refunds NUMERIC,
    discounts NUMERIC,
    net_sales NUMERIC,
    cost_of_goods NUMERIC,
    gross_profit NUMERIC,
    margin NUMERIC,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

```sql
CREATE TABLE daily_sales_summary (
    sales_date DATE PRIMARY KEY,
    gross_profit NUMERIC(12, 2) DEFAULT 0,
    cost_of_goods NUMERIC(12, 2) DEFAULT 0,
    net_sales NUMERIC(12, 2) DEFAULT 0,

    precipitation_sum_mm NUMERIC(10, 2) DEFAULT 0,
    precipitation_hours NUMERIC(10, 2) DEFAULT 0,
    precipitation_probability_max_percent NUMERIC(10, 2) DEFAULT 0,
    weather_code INTEGER,
    weather_code_description VARCHAR(100),
    maximum_temperature_c NUMERIC(10, 2),

    holiday VARCHAR(255),
    month_name VARCHAR(9),
    days_of_week VARCHAR(9),

    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

### Relationship

```text
sales_data
    │
    │ sales_date
    ▼
daily_sales_summary
```

* `sales_data` stores itemized sales records.
* `daily_sales_summary` stores one aggregated record per day.
* `sales_date` identifies the corresponding business day.
* The daily summary is **upserted** when the pipeline runs again.
* No production sales data is included.
