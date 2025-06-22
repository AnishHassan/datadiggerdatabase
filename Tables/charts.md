# Table: `charts`

# Description

Stores saved chart configurations created by users. Each chart includes metadata like title, type, configuration, visibility, and applicable date range.

---

# Schema

| Column Name | Data Type | Null | Default             | Constraints | Description                                                        |
| ----------- | --------- | ---- | ------------------- | ----------- | ------------------------------------------------------------------ |
| id          | UUID      | NO   | `gen_random_uuid()` | Primary Key | Unique identifier for the chart                                    |
| title       | TEXT      | YES  | NULL                |             | Optional title given to the chart                                  |
| description | TEXT      | YES  | NULL                |             | Optional description providing context for the chart               |
| chart_type  | TEXT      | NO   | NULL                |             | Type of chart (e.g., `bar`, `line`, `pie`)                         |
| config      | JSONB     | YES  | NULL                |             | JSON configuration for chart rendering (labels, datasets, options) |
| date_range  | TSRANGE   | YES  | NULL                |             | The time span the chart data covers                                |
| is_public   | BOOL      | YES  | `false`             |             | Indicates whether the chart is publicly accessible                 |
| created_by  | UUID      | YES  | NULL                | Foreign Key | References the user who created the chart                          |
| created_at  | TIMESTAMP | NO   | CURRENT_TIMESTAMP   |             | Timestamp of when the chart was created                            |
| updated_at  | TIMESTAMP | NO   | CURRENT_TIMESTAMP   |             | Timestamp of the last update to the chart                          |

---

# Relationships

| Related Table | Relationship Type | Foreign Key | Description                   |
| ------------- | ----------------- | ----------- | ----------------------------- |
| users         | Many-to-One       | created_by  | A user can create many charts |

---

# Business Rules

* `chart_type` must be specified to define how the chart will be rendered.
* `config` should follow the format expected by the front-end charting library (e.g., Chart.js, ECharts).
* If `is_public = true`, the chart is viewable by all users or externally.
* `date_range` is optional but useful for filtering data over a specific period.

---

# Indexes

| Index Name              | Column      | Type  | Description                                      |
| ----------------------- | ----------- | ----- | ------------------------------------------------ |
| `idx_charts_created_by` | created_by  | BTREE | Speeds up chart retrieval by user                |
| `idx_charts_is_public`  | is_public   | BTREE | Helps filter and list only public charts         |
| `idx_charts_chart_type` | chart_type  | BTREE | Useful for querying charts by visualization type |

---

# Example Row

```json
{
  "id": "83c0f6d4-e9bd-403c-89f4-e902f32c29fd",
  "title": "Weekly Active Users",
  "description": "Line chart showing weekly user activity for Q2",
  "chart_type": "line",
  "config": {
    "labels": ["Week 1", "Week 2", "Week 3"],
    "datasets": [
      {
        "label": "Active Users",
        "data": [120, 150, 180]
      }
    ]
  },
  "date_range": "[2025-04-01,2025-06-30]",
  "is_public": true,
  "created_by": "f2c5e98e-33be-4172-8b3b-bf5944f8235d",
  "created_at": "2025-06-20T09:00:00Z",
  "updated_at": "2025-06-21T08:30:00Z"
}
```
