# Table: `economic_indicator_metadata`

### Description:

This table stores the metadata for economic indicators, providing descriptive and configuration-level data that help define how each indicator behaves, displays, and is grouped across different regions and sources.

This acts as a master reference for indicators used in `economic_calendar_events`.

---

## Schema

| Column Name              | Data Type    | Null | Constraints                  | Description                                                                 |
| ------------------------ | ------------ | ---- | ---------------------------- | --------------------------------------------------------------------------- |
| `indicator_code`         | VARCHAR(64)  | NO   | Primary Key                  | Unique code for the economic indicator (e.g., `GDP_QOQ`, `CPI_YOY`)         |
| `group_code`             | VARCHAR(64)  | YES  | -                            | Code for a group or family of related indicators (e.g., all inflation data) |
| `country_iso2`           | BPCHAR(2)    | NO   | FK → `countries`             | ISO 3166-1 alpha-2 code (e.g., `US`, `JP`, `DE`)                            |
| `source_shortname`       | VARCHAR(64)  | YES  | -                            | Short name for the data source (e.g., `BLS`, `ONS`, `Eurostat`)             |
| `indicator_title`        | VARCHAR(255) | YES  | -                            | Human-readable title of the indicator (e.g., `Gross Domestic Product QoQ`)  |
| `category`               | VARCHAR(64)  | YES  | -                            | Category (e.g., `Inflation`, `Growth`, `Labor Market`)                      |
| `frequency`              | VARCHAR(32)  | YES  | -                            | How often it's released (`Monthly`, `Quarterly`, `Annually`, etc.)          |
| `default_release_time`   | TIME         | YES  | -                            | Standard/local release time                                                 |
| `default_timezone`       | VARCHAR(64)  | YES  | -                            | Timezone for default release time (e.g., `America/New_York`, `UTC`)         |
| `default_impact_level`   | VARCHAR(16)  | YES  | -                            | Typical market impact (`High`, `Medium`, `Low`)                             |
| `unit`                   | VARCHAR(64)  | YES  | -                            | Unit of measurement (e.g., `%`, `Millions`, `Index Points`)                 |
| `preferred_format`       | VARCHAR(32)  | YES  | -                            | Formatting hint (e.g., `percent`, `decimal`, `index`)                       |
| `is_seasonally_adjusted` | BOOLEAN      | YES  | -                            | Whether data is seasonally adjusted                                         |
| `display_order`          | INT4         | YES  | Default: `NULL` or `0`       | Used for UI ordering or prioritization                                      |
| `created_at`             | TIMESTAMP    | YES  | Default: `CURRENT_TIMESTAMP` | Timestamp of creation                                                       |

---

## Key Relationships

* **Joins with `economic_calendar_events`** via `indicator_code`
* **Joins with `countries`** via `country_iso2`

---

## Example Record

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

## Usage Scenarios

* Enrich `economic_calendar_events` with display and formatting metadata.
* Configure alerting or rendering rules based on `default_impact_level`, `timezone`, or `preferred_format`.
* Group indicators in dashboards or reports using `group_code`, `category`, or `frequency`.

---

## Query Examples

**Get all inflation-related indicators for the Eurozone:**

```sql
SELECT indicator_code, indicator_title
FROM economic_indicator_metadata
WHERE category = 'Inflation'
  AND country_iso2 = 'EU';
```

**Find high-impact indicators that are released quarterly:**

```sql
SELECT indicator_code, indicator_title, unit
FROM economic_indicator_metadata
WHERE frequency = 'Quarterly'
  AND default_impact_level = 'High';
```

**Join with calendar events:**

```sql
SELECT e.date, e.title, m.indicator_title, m.unit
FROM economic_calendar_events e
JOIN economic_indicator_metadata m ON e.indicator_code = m.indicator_code
WHERE e.date >= CURRENT_DATE
ORDER BY e.date;
```