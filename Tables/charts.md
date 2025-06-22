# Table: `charts`

---

## Description

Holds saved chart configurations created by users. Charts can be personalized or shared, contain visual metadata, and define how data should be presented. This table powers the chart-building and visualization layer.

---

## Schema

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

## Relationships

| Related Table | Relationship Type | Foreign Key | Description                   |
| ------------- | ----------------- | ----------- | ----------------------------- |
| users         | Many-to-One       | created_by  | A user can create many charts |

---

## Example Record

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

---

## Usage Scenarios

* Store reusable visualizations for dashboards and reports.
* Enable users to create, edit, and manage charts.
* Filter charts by type, creator, or visibility (e.g., “Public Bar Charts”).
* Version control using `updated_at` to track modifications.
* Visual rendering based on the `config` JSON structure.

---

## Business Rules

* `chart_type` is required to determine how the chart is displayed.
* `config` must match the expected schema of the front-end charting engine (e.g., Chart.js, ECharts).
* `is_public = true` makes the chart viewable by others without authentication.
* `created_by` can be `NULL` for system-generated or anonymous charts.
* `date_range` enables filtering data within a specific time span.

---

## Indexes

| Index Name              | Column      | Type  | Description                                      |
| ----------------------- | ----------- | ----- | ------------------------------------------------ |
| `idx_charts_created_by` | created\_by | BTREE | Speeds up chart retrieval by user                |
| `idx_charts_is_public`  | is\_public  | BTREE | Helps filter and list only public charts         |
| `idx_charts_chart_type` | chart\_type | BTREE | Useful for querying charts by visualization type |

---

## Query Examples

### 1. Retrieve all public charts of type `line`:

```sql
SELECT * FROM charts
WHERE is_public = true AND chart_type = 'line'
ORDER BY updated_at DESC;
```

### 2. Find all charts created by a specific user:

```sql
SELECT id, title, chart_type, created_at
FROM charts
WHERE created_by = 'f2c5e98e-33be-4172-8b3b-bf5944f8235d';
```

### 3. Filter charts with a specific date range:

```sql
SELECT id, title
FROM charts
WHERE date_range && '[2025-06-01,2025-06-30]'::tsrange;
```

---

## Insert Example

```sql
INSERT INTO charts (
  id, title, description, chart_type, config, date_range, is_public, created_by, created_at, updated_at
) VALUES (
  gen_random_uuid(),
  'Sales Overview',
  'Quarterly revenue trends',
  'bar',
  '{
    "labels": ["Q1", "Q2", "Q3"],
    "datasets": [{"label": "Revenue", "data": [50000, 70000, 65000]}]
  }'::jsonb,
  '[2025-01-01,2025-09-30]'::tsrange,
  true,
  'f2c5e98e-33be-4172-8b3b-bf5944f8235d',
  CURRENT_TIMESTAMP,
  CURRENT_TIMESTAMP
);
```