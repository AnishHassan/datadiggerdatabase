# Table: `chart_views`

# Description

Records each time a chart is viewed by a user. Useful for analytics, measuring chart popularity, and understanding user engagement trends.

---

# Schema

| Column Name | Data Type | Null | Default             | Constraints | Description                                 |
| ----------- | --------- | ---- | ------------------- | ----------- | ------------------------------------------- |
| id          | UUID      | NO   | `gen_random_uuid()` | Primary Key | Unique identifier for the chart view record |
| chart_id    | UUID      | YES  | NULL                | Foreign Key | References the chart that was viewed        |
| viewer_id   | UUID      | YES  | NULL                | Foreign Key | References the user who viewed the chart    |
| viewed_at   | TIMESTAMP | NO   | CURRENT_TIMESTAMP   |             | Timestamp of when the chart was viewed      |

---

# Relationships

| Related Table | Relationship Type | Foreign Key | Description                         |
| ------------- | ----------------- | ----------- | ----------------------------------- |
| charts        | Many-to-One       | chart_id    | A chart can be viewed by many users |
| users         | Many-to-One       | viewer_id   | A user can view many charts         |

---

# Business Rules

* Each view event is tracked as a separate record for accurate analytics.
* Can be used to generate heatmaps, trending charts, or recommend popular items.
* `viewer_id` can be null if anonymous viewing is allowed.

---

# Indexes

| Index Name               | Column     | Type  | Description                                      |
| ------------------------ | ---------- | ----- | ------------------------------------------------ |
| `idx_chart_views_chart`  | chart_id   | BTREE | Speeds up queries filtering by chart             |
| `idx_chart_views_viewer` | viewer_id  | BTREE | Helps retrieve all views made by a specific user |

---

# Example Row

```json
{
  "id": "c7fb58c0-a425-4f2e-92a7-7390a8c6bca0",
  "chart_id": "faac3492-740f-4f02-9dd8-9d5fcf77d7a3",
  "viewer_id": "3f00ec47-5ba9-4d38-81d7-0fce8fd84e67",
  "viewed_at": "2025-06-21T14:20:00Z"
}
```