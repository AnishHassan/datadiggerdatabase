# Table: `dataset_data_points`

---

# Description

Stores individual data points associated with a dataset. Each point typically represents a (x, y) value on a chart, grouped by `series_name`. These points support dynamic visualizations such as time series, categorical comparisons, and KPI trend monitoring.

---

# Schema

| Column Name   | Data Type | Null | Default             | Constraints | Description                                                     |
| ------------- | --------- | ---- | ------------------- | ----------- | --------------------------------------------------------------- |
| `id`          | UUID      | NO   | `gen_random_uuid()` | Primary Key | Unique identifier for the data point                            |
| `dataset_id`  | UUID      | YES  | NULL                | Foreign Key | References the dataset to which this data point belongs         |
| `x_value`     | TEXT      | YES  | NULL                |             | X-axis value (e.g., date, label, category name)                 |
| `y_value`     | NUMERIC   | YES  | NULL                |             | Y-axis value (e.g., a number associated with the `x_value`)     |
| `series_name` | TEXT      | YES  | NULL                |             | Name of the data series this point belongs to (e.g., "Revenue") |
| `created_at`  | TIMESTAMP | NO   | `CURRENT_TIMESTAMP` |             | Timestamp when this data point was created                      |

---

# Relationships

| Related Table | Relationship Type | Foreign Key  | Description                                              |
| ------------- | ----------------- | ------------ | -------------------------------------------------------- |
| `datasets`    | Many-to-One       | `dataset_id` | A dataset can contain many `dataset_data_points` entries |

---

# Example Record

```json
{
  "id": "af731512-2c3b-4e0d-98e2-99fb132b1b0d",
  "dataset_id": "a3e1c1b9-489f-47b0-a1b1-4e842f64e55f",
  "x_value": "2025-06-01",
  "y_value": 45.8,
  "series_name": "Revenue",
  "created_at": "2025-06-21T14:50:00Z"
}
```

---

# Usage Scenarios

* Visualizing time-series trends (e.g., revenue per day).
* Displaying KPIs grouped by categories (e.g., region-wise conversions).
* Feeding data to dynamic dashboards and reporting tools.
* Supporting multi-series charts by grouping on `series_name`.

---

# Query Examples

# 1. Fetch all data points for a specific dataset

```sql
SELECT x_value, y_value, series_name
FROM dataset_data_points
WHERE dataset_id = 'a3e1c1b9-489f-47b0-a1b1-4e842f64e55f'
ORDER BY series_name, x_value;
```

# 2. Get the latest data point per series in a dataset

```sql
SELECT DISTINCT ON (series_name) *
FROM dataset_data_points
WHERE dataset_id = 'a3e1c1b9-489f-47b0-a1b1-4e842f64e55f'
ORDER BY series_name, x_value DESC;
```

# 3. Aggregate average `y_value` by month per series

```sql
SELECT
  series_name,
  date_trunc('month', x_value::date) AS month,
  AVG(y_value) AS avg_value
FROM dataset_data_points
WHERE dataset_id = 'a3e1c1b9-489f-47b0-a1b1-4e842f64e55f'
GROUP BY series_name, month
ORDER BY month;
```

---

# Insert Example

```sql
INSERT INTO dataset_data_points (
  id,
  dataset_id,
  x_value,
  y_value,
  series_name,
  created_at
) VALUES (
  gen_random_uuid(),
  'a3e1c1b9-489f-47b0-a1b1-4e842f64e55f',
  '2025-06-01',
  45.8,
  'Revenue',
  CURRENT_TIMESTAMP
);
```