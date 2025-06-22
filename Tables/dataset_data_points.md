# Table: `dataset_data_points`

# Description

Stores individual data points associated with datasets. Each point typically represents a value on a chart, categorized by series.

---

# Schema

| Column Name  | Data Type | Null | Default             | Constraints | Description                                                   |
| ------------ | --------- | ---- | ------------------- | ----------- | ------------------------------------------------------------- |
| id           | UUID      | NO   | `gen_random_uuid()` | Primary Key | Unique identifier for the data point                          |
| dataset_id   | UUID      | YES  | NULL                | Foreign Key | References the dataset to which this data point belongs       |
| x_value      | TEXT      | YES  | NULL                |             | X-axis value (e.g., date, category, label)                    |
| y_value      | NUMERIC   | YES  | NULL                |             | Y-axis value (e.g., numerical metric for the corresponding x) |
| series_name  | TEXT      | YES  | NULL                |             | Name of the data series this point belongs to                 |
| created_at   | TIMESTAMP | NO   | `CURRENT_TIMESTAMP` |             | Timestamp when this data point was created                    |

---

# Relationships

| Related Table | Relationship Type | Foreign Key | Description                                    |
| ------------- | ----------------- | ----------- | ---------------------------------------------- |
| datasets      | Many-to-One       | dataset_id  | A dataset can have many associated data points |

---

# Business Rules

* A dataset can contain multiple data points across different series.
* `x_value`, `y_value`, and `series_name` together define a unique point in most charting scenarios, though not enforced at DB level unless explicitly indexed.
* Used to dynamically render charts based on dataset content.


# Example Row

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