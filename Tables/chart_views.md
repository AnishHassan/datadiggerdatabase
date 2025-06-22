# Table: `chart_views`

---

# Description

Tracks every instance when a chart is viewed by a user. Supports analytics use cases such as tracking popularity, measuring engagement, and driving personalized or trending content.

---

# Schema

| Column Name | Data Type | Null | Default             | Constraints | Description                                 |
| ----------- | --------- | ---- | ------------------- | ----------- | ------------------------------------------- |
| id          | UUID      | NO   | `gen_random_uuid()` | Primary Key | Unique identifier for the chart view record |
| chart_id    | UUID      | YES  | NULL                | Foreign Key | References the chart that was viewed        |
| viewer_id   | UUID      | YES  | NULL                | Foreign Key | References the user who viewed the chart    |
| viewed_at   | TIMESTAMP | NO   | `CURRENT_TIMESTAMP` |             | Timestamp of when the chart was viewed      |

---

# Relationships

| Related Table | Relationship Type | Foreign Key | Description                         |
| ------------- | ----------------- | ----------- | ----------------------------------- |
| charts        | Many-to-One       | chart_id    | A chart can be viewed by many users |
| users         | Many-to-One       | viewer_id   | A user can view many charts         |

---

# Example Record

```json
{
  "id": "c7fb58c0-a425-4f2e-92a7-7390a8c6bca0",
  "chart_id": "faac3492-740f-4f02-9dd8-9d5fcf77d7a3",
  "viewer_id": "3f00ec47-5ba9-4d38-81d7-0fce8fd84e67",
  "viewed_at": "2025-06-21T14:20:00Z"
}
```

---

# Usage Scenarios

* Measure chart popularity based on number of views.
* Identify most-engaged users or most-viewed charts over time.
* Power recommendations (e.g., "Most Viewed Charts This Week").
* Generate user-level insights into viewing behavior.
* Build analytics dashboards with trends, time-based patterns, and peak usage.

---

# Query Examples

# 1. Get view count per chart:

```sql
SELECT chart_id, COUNT(*) AS total_views
FROM chart_views
GROUP BY chart_id
ORDER BY total_views DESC;
```

# 2. Get all charts viewed by a specific user:

```sql
SELECT chart_id, viewed_at
FROM chart_views
WHERE viewer_id = '3f00ec47-5ba9-4d38-81d7-0fce8fd84e67'
ORDER BY viewed_at DESC;
```

# 3. Count anonymous views for a specific chart:

```sql
SELECT COUNT(*) AS anonymous_views
FROM chart_views
WHERE chart_id = 'faac3492-740f-4f02-9dd8-9d5fcf77d7a3'
  AND viewer_id IS NULL;
```

---

# Insert Example

```sql
INSERT INTO chart_views (
  id, chart_id, viewer_id, viewed_at
)
VALUES (
  gen_random_uuid(),
  'faac3492-740f-4f02-9dd8-9d5fcf77d7a3',
  '3f00ec47-5ba9-4d38-81d7-0fce8fd84e67',
  CURRENT_TIMESTAMP
);
```

> If anonymous views are allowed:

```sql
INSERT INTO chart_views (
  id, chart_id, viewer_id, viewed_at
)
VALUES (
  gen_random_uuid(),
  'faac3492-740f-4f02-9dd8-9d5fcf77d7a3',
  NULL,
  CURRENT_TIMESTAMP
);
```