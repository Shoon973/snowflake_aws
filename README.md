# Airbnb Data Engineering Project | Snowflake + dbt

## Project Overview

This project demonstrates an end-to-end data transformation workflow using **Snowflake, dbt (Data Build Tool), and Python**.

The project focuses on transforming Airbnb booking, listing, and host data into structured datasets that can support business analysis and reporting.

Using dbt, I implemented a layered data architecture (Bronze, Silver, and Gold) to organize raw data ingestion, data cleaning, transformation, and analytical modelling.

The goal is to build a scalable and maintainable data pipeline that converts raw Airbnb data into analytics-ready datasets.

## Technologies Used

| Technology | Purpose |
|---|---|
| Snowflake | Cloud data warehouse |
| dbt Core | Data transformation and modelling |
| dbt-snowflake | Snowflake integration |
| Python 3.12+ | Project environment |
| SQL | Data transformation and querying |
| Jinja | Dynamic SQL and reusable macros |
| uv | Python dependency management |

## Project Architecture

The project follows a Medallion Architecture with three main layers:

### 1. Bronze Layer – Raw Data

The Bronze layer loads raw data from Snowflake staging tables.

Source tables include:
- Listings
- Bookings
- Hosts

**Implementation:**
- Incremental models
- Raw data extraction
- Creation of Bronze tables

### 2. Silver Layer – Data Transformation

The Silver layer cleans and transforms the Bronze datasets.

Transformations include:
- Selecting relevant columns
- Standardizing host names
- Creating host response quality categories
- Calculating total booking amounts
- Categorizing listing prices

Business logic is implemented using reusable dbt macros.

### 3. Gold Layer – Business Analytics

The Gold layer combines transformed datasets into analytics-ready models.

It includes:

- **OBT (One Big Table):** Combines booking, listing, and host information into a consolidated dataset.
- **Fact Model:** Builds a fact-oriented dataset by joining the OBT with listing and host dimension tables.
- **Snapshots:** Tracks historical changes in listings, bookings, and host records.

## Data Pipeline

The overall workflow:

```text
        Snowflake
            |
      Staging Tables
            |
            v
      Bronze Layer
      Raw Data Models
            |
            v
      Silver Layer
    Cleaned & Transformed
            |
            v
       Gold Layer
       /        \
      v          v
     OBT       Fact Model
      |
      v
 Analytics-Ready Data
```

## Key Features

### Incremental Data Loading

Implemented incremental dbt models to process newly created records based on the `CREATED_AT` timestamp.

### Data Transformation

Used SQL and Jinja to implement reusable business logic and data cleaning.

### Reusable dbt Macros

Developed custom macros to simplify repeated calculations:

- `multiply`: Calculates and rounds multiplication results.
- `tag`: Categorizes listing prices into LOW, MEDIUM, and HIGH.
- `trimmer`: Intended for text standardization.

### Historical Data Tracking

Implemented dbt snapshots to track historical changes in:

- Listings
- Bookings
- Hosts

### Data Quality Testing

Included SQL-based data quality checks to identify booking records with booking amounts below a specified threshold.

## Project Structure

```text
snowflake_aws/
│
├── aws_dbt_snowflake_project/
│   │
│   ├── models/
│   │   ├── bronze/
│   │   │   ├── bronze_bookings.sql
│   │   │   ├── bronze_hosts.sql
│   │   │   └── bronze_listings.sql
│   │   │
│   │   ├── silver/
│   │   │   ├── silver_bookings.sql
│   │   │   ├── silver_hosts.sql
│   │   │   └── silver_listings.sql
│   │   │
│   │   ├── gold/
│   │   │   ├── obt.sql
│   │   │   ├── fact.sql
│   │   │   └── ephemeral/
│   │   │
│   │   └── sources/
│   │       └── sources.yml
│   │
│   ├── macros/
│   │   ├── multiply.sql
│   │   ├── tag.sql
│   │   ├── trimmer.sql
│   │   └── generate_schema_name.sql
│   │
│   ├── snapshots/
│   │   ├── dim_listings.yml
│   │   ├── dim_bookings.yml
│   │   └── dim_hosts.yml.yml
│   │
│   ├── tests/
│   ├── analyses/
│   ├── seeds/
│   ├── profiles.yml
│   └── dbt_project.yml
│
├── src/
│   └── snowflake/
│
├── pyproject.toml
└── README.md
```

## Installation and Setup

### Prerequisites

Install the following:

- Python 3.12 or later
- Snowflake account
- uv package manager
- dbt Core
- dbt-snowflake adapter

### 1. Clone the Repository

```bash
git clone <your-github-repository-url>
cd snowflake_aws
```

### 2. Install Dependencies

```bash
uv sync
```

### 3. Configure Snowflake Connection

Configure your dbt profile in:

```text
~/.dbt/profiles.yml
```

Example configuration:

```yaml
aws_dbt_snowflake_project:
  target: dev
  outputs:
    dev:
      type: snowflake
      account: YOUR_ACCOUNT
      user: YOUR_USERNAME
      password: YOUR_PASSWORD
      role: YOUR_ROLE
      database: AIRBNB
      warehouse: COMPUTE_WH
      schema: dbt_schema
      threads: 4
```

Use environment variables or a secure credential-management method for passwords. Never commit real credentials to GitHub.

### 4. Test Snowflake Connection

Navigate to the dbt project directory:

```bash
cd aws_dbt_snowflake_project
```

Run:

```bash
uv run dbt debug
```

### 5. Install dbt Dependencies

```bash
uv run dbt deps
```

### 6. Run dbt Models

```bash
uv run dbt run
```

### 7. Execute Data Quality Tests

```bash
uv run dbt test
```

### 8. Execute Snapshots

```bash
uv run dbt snapshot
```

### 9. Generate dbt Documentation

```bash
uv run dbt docs generate
uv run dbt docs serve
```

## Business Value

This project demonstrates how raw Airbnb data can be transformed into structured datasets for business intelligence and analytics.

The resulting data models can support analysis of:

- Booking performance
- Listing characteristics
- Host performance
- Property pricing
- Booking revenue
- Historical changes in business data

The Gold layer provides a foundation for connecting analytical datasets to reporting tools such as Power BI or Tableau.

## Skills Demonstrated

- SQL and advanced data transformation
- Snowflake data warehousing
- dbt Core and dbt-snowflake
- Medallion Architecture
- Incremental data loading
- Data modelling
- Fact and dimension modelling
- Jinja templating
- Custom dbt macros
- dbt snapshots
- Data quality testing
- Python environment management

## Conclusion

This project strengthened my practical understanding of modern data engineering workflows and cloud data warehousing.

By combining Snowflake and dbt, I developed a structured transformation pipeline that separates raw data, cleaned datasets, and business-ready analytical models.

It demonstrates my ability to work with SQL-based transformations, modular data modelling, incremental processing, and historical data tracking.

---

**Project Category:** Data Engineering / Analytics Engineering

**Primary Technologies:** Snowflake | dbt | SQL | Python

**Author:** Your Name
