# Table: `chart_saves`

---

# Description

Tracks which users have saved which charts for future access or reference. Enables personalized dashboards, quick retrieval of frequently used charts, and insight into user preferences.

---

# Schema

| Column Name | Data Type | Null | Default             | Constraints | Description                                   |
| ----------- | --------- | ---- | ------------------- | ----------- | --------------------------------------------- |
| id          | UUID      | NO   | `gen_random_uuid()` | Primary Key | Unique identifier for the chart save entry    |
| user_id     | UUID      | YES  | NULL                | Foreign Key | References the user who saved the chart       |
| chart_id    | UUID      | YES  | NULL                | Foreign Key | References the chart that was saved           |
| saved_at    | TIMESTAMP | NO   | `CURRENT_TIMESTAMP` |             | Timestamp indicating when the chart was saved |

---

# Relationships

| Related Table | Relationship Type | Foreign Key | Description                            |
| ------------- | ----------------- | ----------- | -------------------------------------- |
| users         | Many-to-One       | user_id     | A user can save multiple charts        |
| charts        | Many-to-One       | chart_id    | A chart can be saved by multiple users |

---

# Example Record

```json
{
  "id": "61a19c74-2c91-4b6f-b77f-8bdf92d91df2",
  "user_id": "3f00ec47-5ba9-4d38-81d7-0fce8fd84e67",
  "chart_id": "faac3492-740f-4f02-9dd8-9d5fcf77d7a3",
  "saved_at": "2025-06-21T12:45:00Z"
}
```

---

# Usage Scenarios

* Allow users to bookmark or pin charts for quick access later.
* Enable “Saved Charts” views in user dashboards.
* Power recommendation or retargeting features based on user interest.
* Support analytics to measure chart reuse frequency.

---

# Query Examples

# 1. Get all charts saved by a specific user:

```sql
SELECT chart_id
FROM chart_saves
WHERE user_id = '3f00ec47-5ba9-4d38-81d7-0fce8fd84e67';
```

# 2. Get number of saves per chart:

```sql
SELECT chart_id, COUNT(*) AS total_saves
FROM chart_saves
GROUP BY chart_id
ORDER BY total_saves DESC;
```

# 3. Check if a chart is already saved by a user:

```sql
SELECT 1
FROM chart_saves
WHERE user_id = '3f00ec47-5ba9-4d38-81d7-0fce8fd84e67'
  AND chart_id = 'faac3492-740f-4f02-9dd8-9d5fcf77d7a3';
```

---

# Insert Example

```sql
INSERT INTO chart_saves (
  id, user_id, chart_id, saved_at
)
VALUES (
  gen_random_uuid(),
  '3f00ec47-5ba9-4d38-81d7-0fce8fd84e67',
  'faac3492-740f-4f02-9dd8-9d5fcf77d7a3',
  CURRENT_TIMESTAMP
)
ON CONFLICT (user_id, chart_id) DO NOTHING;
```
