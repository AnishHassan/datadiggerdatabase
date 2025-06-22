# Table: `economic_indicator_values`

### **Description**

This table stores the actual numerical values reported for a specific economic indicator during a particular calendar event. It links calendar events (e.g., CPI release on a specific date) with the recorded results, enabling structured access to economic data.

---

## Schema

| Column Name      | Data Type     | Null | Constraints                                   | Description
| ---------------- | ------------- | ---- | ----------------------------------------------| ------------------------------------------------------------------ |
| `id`             | INT4          | NO   | Primary Key, Auto-Increment                   | Unique row ID                                                      |
| `event_id`       | INT4          | NO   | Foreign Key → `economic_calendar_events(id)`  | Associated economic calendar event                                 |
| `indicator_code` | VARCHAR(64)   | NO   | Foreign Key → `economic_indicator_metadata(indicator_code)` | Code identifying the economic indicator              |
| `actual_value`   | NUMERIC(16,4) | YES  | –                                             | The actual reported value for the indicator (e.g., 3.2500 = 3.25%) |
| `unit`           | VARCHAR(32)   | YES  | –                                             | Unit of measurement (overrides metadata unit if provided)          |
| `created_at`     | TIMESTAMP     | YES  | Default: `CURRENT_TIMESTAMP`                  | Timestamp when the record was created                              |
| `updated_at`     | TIMESTAMP     | YES  | Default: `CURRENT_TIMESTAMP`                  | Timestamp when the record was last updated                         |

---

## Indexes

| Index Name            | Columns                        | Type  | Description                                                                  |
| --------------------- | ------------------------------ | ----- | ---------------------------------------------------------------------------- |
| `unique_event_metric` | (`event_id`, `indicator_code`) | BTREE | Ensures each event has at most one value per indicator (enforces uniqueness) |

> 💡 Useful for validating that no duplicate entries are recorded for the same indicator/event combination.

---

## Key Relationships

* `event_id` → `economic_calendar_events.id`
* `indicator_code` → `economic_indicator_metadata.indicator_code`

---

## Example Record

```json
{
  "id": 284,
  "event_id": 1123,
  "indicator_code": "CPI_YOY",
  "actual_value": 3.2500,
  "unit": "%",
  "created_at": "2025-06-21 09:00:00",
  "updated_at": "2025-06-21 09:00:00"
}
```

---

## Usage Scenarios

* Displaying real-time and historical economic indicator values
* Feeding data into charts and dashboards showing macroeconomic trends
* Comparing actual values against forecasts or prior results
* Enabling economic model inputs (e.g., inflation tracking, GDP analysis)

---

## Query Examples

### Latest reported values across indicators

```sql
SELECT 
  i.indicator_code, 
  i.actual_value, 
  i.unit, 
  e.date
FROM economic_indicator_values i
JOIN economic_calendar_events e ON i.event_id = e.id
ORDER BY e.date DESC
LIMIT 10;
```

### Time series for a specific indicator (e.g., GDP_QOQ)

```sql
SELECT 
  e.date, 
  i.actual_value
FROM economic_indicator_values i
JOIN economic_calendar_events e ON i.event_id = e.id
WHERE i.indicator_code = 'GDP_QOQ'
  AND e.date >= CURRENT_DATE - INTERVAL '1 year'
ORDER BY e.date;
```