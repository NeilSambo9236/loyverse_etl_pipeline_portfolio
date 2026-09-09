# Automated Loyverse Sales ETL Pipeline

A Python-based ETL pipeline built for a small food business to automate sales data collection and preserve historical records beyond Loyverse's one-month data retention period.

> **Note:** This is a portfolio overview of a real client project. The production source code and business data are kept private.

## Why It Was Made

Loyverse only retains a limited amount of historical sales data, while the business needed a way to keep its records over time.

The pipeline was created to automate the collection of sales data and store it in a persistent PostgreSQL database instead of relying on manual spreadsheet work.

## How It Works

```text
Loyverse
   ↓
Extract
Python + Playwright / REST API
   ↓
Transform
Clean and organize sales data
   ↓
Load
Neon PostgreSQL
   ↓
Daily Sales Summary
```

The pipeline runs automatically through **GitHub Actions** and stores the collected data in PostgreSQL for long-term use.

## Database Design

The sales data is stored in a centralized table containing information such as:

* Sales date
* Item and category
* Items sold
* Gross sales
* Discounts
* Net sales
* Cost of goods
* Gross profit
* Margin

Daily summaries are also generated from the stored sales data.

## Tech Stack

**Python · Playwright · REST APIs · PostgreSQL · Neon · Git · GitHub Actions**

## Architecture

![Pipeline Architecture](architecture.png)

The production implementation remains private because it contains client-specific code and data. This repository only presents the project's architecture and database design.
