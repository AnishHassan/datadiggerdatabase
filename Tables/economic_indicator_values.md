# **Table: `economic_indicator_values`**

---

## **Description**

The `economic_indicator_values` table stores the actual numerical values reported for economic indicators during specific calendar events. It links an `indicator_code` to an `event_id`, representing the observed value released by official sources (e.g., CPI, GDP, Unemployment) on specific dates. This table supports time-series analysis, comparison against forecasts, and visualizations of macroeconomic trends.

---

## **Schema**

| Column Name      | Data Type     | Nullable | Constraints                       | Description  |
| ---------------- | ------------- | -------- | -------------------------------------------------| ----------------------------------------------------------- |
| `id`             | INT4          | NO       | Primary Key, Auto-increment                      | Unique identifier for each value record.                    |
| `event_id`       | INT4          | NO       | FK → `economic_calendar_events.id`               | References the associated economic calendar event.          |
| `indicator_code` | VARCHAR(64)   | NO       | FK → `economic_indicator_metadata.indicator_code`| Identifier of the indicator the value is tied to.           |
| `actual_value`   | NUMERIC(16,4) | YES      | -                                                | The reported value of the indicator (e.g., 3.2500 = 3.25%). |
| `unit`           | VARCHAR(32)   | YES      | -                                                | Unit of measurement (overrides metadata if specified).      |
| `created_at`     | TIMESTAMP     | YES      | Default: `CURRENT_TIMESTAMP`                     | Timestamp when the value was first recorded.                |
| `updated_at`     | TIMESTAMP     | YES      | Default: `CURRENT_TIMESTAMP`                     | Timestamp when the value was last modified.                 |

---

## **Relationships**

* **`event_id`** → `economic_calendar_events.id`
* **`indicator_code`** → `economic_indicator_metadata.indicator_code`

---

## **Indexes**

| Index Name            | Columns                        | Type  | Purpose                                                                           |
| --------------------- | ------------------------------ | ----- | --------------------------------------------------------------------------------- |
| `unique_event_metric` | (`event_id`, `indicator_code`) | BTREE | Ensures one value per indicator per event (prevents duplicate reporting entries). |

---

## **Example Record**

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

## **Usage Scenarios**

* Rendering actual reported values for upcoming or historical economic events
* Visualizing macroeconomic indicators across time (e.g., CPI trendlines)
* Comparing reported `actual_value` with `forecast` or `previous` values (from events)
* Feeding quantitative models or dashboards with structured economic data
* Enabling backend data pipelines for analytics, scoring, or alerting

---

## **Query Examples**

### Latest reported values for all indicators

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

---

### Time series of values for a specific indicator (e.g., `GDP_QOQ`)

```sql
SELECT 
  e.date, 
  i.actual_value
FROM economic_indicator_values i
JOIN economic_calendar_events e ON i.event_id = e.id
WHERE i.indicator_code = 'GDP_QOQ'
  AND e.date >= CURRENT_DATE - INTERVAL '1 year'
ORDER BY e.date ASC;
```

---

### Compare actual vs. forecasted values

```sql
SELECT 
  e.date,
  e.title,
  i.actual_value,
  e.forecast_value,
  (i.actual_value - e.forecast_value) AS surprise
FROM economic_indicator_values i
JOIN economic_calendar_events e ON i.event_id = e.id
WHERE i.indicator_code = 'CPI_YOY'
ORDER BY e.date DESC;
```

---

## Insert Example**

```sql
INSERT INTO economic_indicator_values (
  event_id,
  indicator_code,
  actual_value,
  unit
) VALUES (
  1123,
  'CPI_YOY',
  3.2500,
  '%'
);
`