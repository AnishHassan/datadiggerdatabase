# Table: `commodity_calendar_events`

---

# Description

Stores calendar-based economic events related to commodities, such as inventories, production reports, and other time-sensitive data. Each record includes metadata (region, source, provider), as well as the forecasted (`survey_value`), actual, prior, and revised values for comparative analysis. Commonly used for visualizing commodity movements, macroeconomic monitoring, and market impact assessments.

---

# Schema

| Column Name       | Data Type    | Null | Default             | Constraints | Description                                                    |
| ----------------- | ------------ | ---- | ------------------- | ----------- | -------------------------------------------------------------- |
| `id`              | INT4         | NO   | -                   | Primary Key | Unique identifier for the commodity event                      |
| `country_iso2`    | BPCHAR(2)    | YES  | -                   | -           | ISO 3166-1 alpha-2 country code (e.g., `US`, `CA`)             |
| `region`          | VARCHAR(64)  | YES  | -                   | -           | Sub-national region or economic zone (e.g., `EIA`)             |
| `date`            | DATE         | NO   | -                   | -           | Date of the event                                              |
| `time`            | TIME         | YES  | -                   | -           | Time of the event (24h format, if specified)                   |
| `title`           | VARCHAR(255) | YES  | -                   | -           | Descriptive title of the event (e.g., `Crude Oil Inventories`) |
| `impact`          | TEXT         | YES  | `'Low'`             | -           | Event impact level (`High`, `Medium`, `Low`)                   |
| `survey_value`    | VARCHAR(64)  | YES  | -                   | -           | Forecasted or expected value                                   |
| `actual_value`    | VARCHAR(50)  | YES  | -                   | -           | Reported value after the event                                 |
| `prior_value`     | VARCHAR(64)  | YES  | -                   | -           | Value reported in the prior period                             |
| `revision_value`  | VARCHAR(64)  | YES  | -                   | -           | Revised value from the previously reported figure              |
| `parent_event_id` | INT4         | YES  | -                   | FK (self)   | Optional link to a parent event (hierarchical structure)       |
| `created_at`      | TIMESTAMPTZ  | YES  | `CURRENT_TIMESTAMP` | -           | Timestamp when the record was inserted                         |
| `unit`            | VARCHAR(20)  | YES  | -                   | -           | Unit of measurement (e.g., `barrels`, `tons`)                  |
| `source_url`      | TEXT         | YES  | -                   | -           | Link to the original data source                               |
| `data_provider`   | VARCHAR(50)  | YES  | -                   | -           | Data provider (e.g., `EIA`, `USDA`)                            |
| `data_type`       | VARCHAR(50)  | YES  | -                   | -           | Type of economic data (e.g., `Inventory`, `Production`)        |

---

# Relationships

| Relationship                | Type           | Description                                                      |
| --------------------------- | -------------- | ---------------------------------------------------------------- |
| `parent_event_id` → `id`    | Self Join      | Optional parent-child link between related events                |
| May link to `charts.config` | JSON reference | Charts may reference events by `title`, `data_type`, or `region` |

> You can create a `chart_data_sources` linking table to formally associate this data with the `charts` table.

---

# Example Record

```json
{
  "id": 9812,
  "country_iso2": "US",
  "region": "EIA",
  "date": "2025-06-19",
  "time": "14:30:00",
  "title": "Crude Oil Inventories",
  "impact": "High",
  "survey_value": "-1.2M",
  "actual_value": "-2.3M",
  "prior_value": "3.0M",
  "revision_value": "2.8M",
  "parent_event_id": null,
  "created_at": "2025-06-19T14:31:00Z",
  "unit": "barrels",
  "source_url": "https://www.eia.gov/petroleum",
  "data_provider": "EIA",
  "data_type": "Inventory"
}
```

---

# Usage Scenarios

Market Analysis : Track discrepancies between forecasted and actual commodity data to assess market reactions.
Data Visualization : Build time series dashboards for crude oil, natural gas, or agricultural reports.
Impact Assessment : Identify high-impact events for event-driven trading or economic forecasting.
Revision Tracking : Monitor updates to previously released figures to reassess historical accuracy.
Data Aggregation : Group events by region, provider, or type for comparative studies across sources.

---

# Query Examples

# 1. Get All Upcoming High-Impact Events

```sql
SELECT *
FROM commodity_calendar_events
WHERE impact = 'High'
  AND date >= CURRENT_DATE
ORDER BY date, time;
```

---

# 2. Compare Actual vs Survey for Crude Oil Events

```sql
SELECT title, date, survey_value, actual_value
FROM commodity_calendar_events
WHERE title ILIKE '%Crude Oil%'
ORDER BY date DESC;
```

---

# 3. Revised Events in the Last 30 Days

```sql
SELECT *
FROM commodity_calendar_events
WHERE revision_value IS NOT NULL
  AND date >= CURRENT_DATE - INTERVAL '30 days';
```

---

# 4. Aggregated Impact Counts by Type

```sql
SELECT data_type, impact, COUNT(*) AS event_count
FROM commodity_calendar_events
GROUP BY data_type, impact
ORDER BY data_type, impact;
```

---

# 5. Time Series of Actual Inventory for a Region

```sql
SELECT date, actual_value
FROM commodity_calendar_events
WHERE data_type = 'Inventory'
  AND region = 'EIA'
  AND actual_value IS NOT NULL
ORDER BY date;
```

---

# Insert Example

```sql
INSERT INTO commodity_calendar_events (
  country_iso2, region, date, time, title, impact,
  survey_value, actual_value, prior_value, revision_value,
  unit, source_url, data_provider, data_type
) VALUES (
  'US', 'EIA', '2025-06-19', '14:30:00', 'Crude Oil Inventories', 'High',
  '-1.2M', '-2.3M', '3.0M', '2.8M',
  'barrels', 'https://www.eia.gov/petroleum', 'EIA', 'Inventory'
);
```