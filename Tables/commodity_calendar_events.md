# Table: `commodity_calendar_events`

### Description:

Stores calendar-based economic events related to commodities, including forecasted (survey), actual, prior, and revised values, with metadata like source, provider, and region.

---

## Schema

| Column Name       | Data Type    | Null | Default             | Constraints | Description                                                 |
| ----------------- | ------------ | ---- | ------------------- | ----------- | ----------------------------------------------------------- |
| id                | INT4         | NO   | -                   | Primary Key | Unique identifier for the commodity event                   |
| country_iso2      | BPCHAR(2)    | YES  | -                   | -           | Two-letter ISO country code (e.g., `US`, `CA`)              |
| region            | VARCHAR(64)  | YES  | -                   | -           | Sub-national region or economic zone                        |
| date              | DATE         | NO   | -                   | -           | Date of the event                                           |
| time              | TIME         | YES  | -                   | -           | Time of the event (if specified)                            |
| title             | VARCHAR(255) | YES  | -                   | -           | Descriptive title of the event                              |
| impact            | TEXT         | YES  | `'Low'`             | -           | Event impact level (e.g., `High`, `Medium`, `Low`)          |
| survey_value      | VARCHAR(64)  | YES  | -                   | -           | Forecasted or expected value                                |
| actual_value      | VARCHAR(50)  | YES  | -                   | -           | Realized or reported value                                  |
| prior_value       | VARCHAR(64)  | YES  | -                   | -           | Value from the previous period                              |
| revision_value    | VARCHAR(64)  | YES  | -                   | -           | Revised value from prior release                            |
| parent_event_id   | INT4         | YES  | -                   | -           | Optional reference to a parent event                        |
| created_at        | TIMESTAMPTZ  | YES  | `CURRENT_TIMESTAMP` | -           | Timestamp when the event was inserted                       |
| unit              | VARCHAR(20)  | YES  | -                   | -           | Measurement unit of the values (e.g., barrels, metric tons) |
| source_url        | TEXT         | YES  | -                   | -           | Link to the official source of the data                     |
| data_provider     | VARCHAR(50)  | YES  | -                   | -           | Organization or service that provides the data              |
| data_type         | VARCHAR(50)  | YES  | -                   | -           | Type of data (e.g., `Inventory`, `Production`, etc.)        |


## Notes

* `impact` is typically categorized by severity to inform traders or analysts.
* `survey_value`, `actual_value`, `prior_value`, and `revision_value` help users evaluate how actual outcomes differ from expectations.
* Useful for visualizing time series of commodity-related metrics like crude oil inventories or agricultural yields.

---

## Example Row

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

## Query Examples

**Get all upcoming high-impact commodity events:**

```sql
SELECT *
FROM commodity_calendar_events
WHERE impact = 'High'
  AND date >= CURRENT_DATE
ORDER BY date, time;
```

**Compare actual vs. survey values for a specific title:**

```sql
SELECT title, date, survey_value, actual_value
FROM commodity_calendar_events
WHERE title ILIKE '%Crude Oil%'
ORDER BY date DESC;
```

**Get revised events for the last 30 days:**

```sql
SELECT *
FROM commodity_calendar_events
WHERE revision_value IS NOT NULL
  AND date >= CURRENT_DATE - INTERVAL '30 days';
```

---

## Insert Example

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