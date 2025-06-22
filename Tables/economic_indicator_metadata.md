# **Table: `economic_indicator_metadata`**

---

## **Description**

The `economic_indicator_metadata` table stores metadata and configuration details for economic indicators. It acts as a master reference for interpreting and displaying economic data across countries and sources. This metadata defines characteristics such as frequency, units, category, display preferences, and typical release behavior for indicators used in economic calendar events.

---

## **Schema**

| Column Name              | Data Type    | Nullable | Constraints               | Description                                                                |
| ------------------------ | ------------ | -------- | ------------------------- | -------------------------------------------------------------------------- |
| `indicator_code`         | VARCHAR(64)  | NO       | Primary Key               | Unique identifier for the economic indicator (e.g., `GDP_QOQ`, `CPI_YOY`). |
| `group_code`             | VARCHAR(64)  | YES      | -                         | Logical group for related indicators (e.g., `INFLATION`).                  |
| `country_iso2`           | BPCHAR(2)    | NO       | FK → `countries.iso2`     | ISO 3166-1 alpha-2 country code (e.g., `US`, `JP`).                        |
| `source_shortname`       | VARCHAR(64)  | YES      | -                         | Abbreviated name of the data source (e.g., `BLS`, `ONS`).                  |
| `indicator_title`        | VARCHAR(255) | YES      | -                         | Human-friendly title for the indicator.                                    |
| `category`               | VARCHAR(64)  | YES      | -                         | General category (e.g., `Growth`, `Inflation`, `Labor Market`).            |
| `frequency`              | VARCHAR(32)  | YES      | -                         | Release frequency (`Monthly`, `Quarterly`, etc.).                          |
| `default_release_time`   | TIME         | YES      | -                         | Standard time of release in local time.                                    |
| `default_timezone`       | VARCHAR(64)  | YES      | -                         | Timezone for `default_release_time` (e.g., `America/New_York`).            |
| `default_impact_level`   | VARCHAR(16)  | YES      | CHECK (High, Medium, Low) | Typical market impact level.                                               |
| `unit`                   | VARCHAR(64)  | YES      | -                         | Unit of measurement (e.g., `%`, `Index Points`).                           |
| `preferred_format`       | VARCHAR(32)  | YES      | -                         | Output formatting hint (e.g., `percent`, `decimal`).                       |
| `is_seasonally_adjusted` | BOOLEAN      | YES      | -                         | Flag indicating seasonal adjustment.                                       |
| `display_order`          | INT          | YES      | `0`                       | UI display priority or ordering.                                           |
| `created_at`             | TIMESTAMP    | YES      | `CURRENT_TIMESTAMP`       | Record creation timestamp.                                                 |

---

## **Relationships**

* **`economic_calendar_events.indicator_code`** → **`economic_indicator_metadata.indicator_code`**
* **`economic_calendar_events.country_iso2`** → **`economic_indicator_metadata.country_iso2`**
* **`countries.iso2`** → **`economic_indicator_metadata.country_iso2`** (foreign key reference)

---

## **Example Record**

```json
{
  "indicator_code": "CPI_YOY",
  "group_code": "INFLATION",
  "country_iso2": "US",
  "source_shortname": "BLS",
  "indicator_title": "Consumer Price Index (YoY)",
  "category": "Inflation",
  "frequency": "Monthly",
  "default_release_time": "08:30:00",
  "default_timezone": "America/New_York",
  "default_impact_level": "High",
  "unit": "%",
  "preferred_format": "percent",
  "is_seasonally_adjusted": true,
  "display_order": 2,
  "created_at": "2025-06-21 14:33:00"
}
```

---

## **Usage Scenarios**

* Enhance `economic_calendar_events` with display titles, formatting, and unit metadata.
* Drive indicator visualizations grouped by `category`, `group_code`, or `frequency`.
* Control rendering and impact styling in dashboards based on `default_impact_level`.
* Support region-specific configurations for indicators across different time zones and data sources.

---

## **Query Examples**

### 1. **List all inflation-related indicators for the Eurozone**

```sql
SELECT indicator_code, indicator_title
FROM economic_indicator_metadata
WHERE category = 'Inflation'
  AND country_iso2 = 'EU';
```

---

### 2. **Fetch high-impact quarterly indicators**

```sql
SELECT indicator_code, indicator_title, unit
FROM economic_indicator_metadata
WHERE frequency = 'Quarterly'
  AND default_impact_level = 'High';
```

---

### 3. **Get upcoming calendar events with metadata**

```sql
SELECT 
  e.date, 
  e.title AS event_title, 
  m.indicator_title, 
  m.unit, 
  m.default_impact_level
FROM economic_calendar_events e
JOIN economic_indicator_metadata m 
  ON e.indicator_code = m.indicator_code 
 AND e.country_iso2 = m.country_iso2
WHERE e.date >= CURRENT_DATE
ORDER BY e.date;
```

---

### 4. **Detect override of default release times**

```sql
SELECT 
  e.indicator_code,
  e.date,
  e.time AS event_time,
  m.default_release_time,
  m.default_timezone
FROM economic_calendar_events e
JOIN economic_indicator_metadata m 
  ON e.indicator_code = m.indicator_code 
 AND e.country_iso2 = m.country_iso2
WHERE e.time IS NOT NULL
  AND e.time <> m.default_release_time;
```

---

## **Insert Example**

```sql
INSERT INTO economic_indicator_metadata (
  indicator_code,
  group_code,
  country_iso2,
  source_shortname,
  indicator_title,
  category,
  frequency,
  default_release_time,
  default_timezone,
  default_impact_level,
  unit,
  preferred_format,
  is_seasonally_adjusted,
  display_order
) VALUES (
  'UNEMPLOYMENT_RATE',
  'LABOR',
  'US',
  'BLS',
  'Unemployment Rate',
  'Labor Market',
  'Monthly',
  '08:30:00',
  'America/New_York',
  'High',
  '%',
  'percent',
  true,
  3
);
```