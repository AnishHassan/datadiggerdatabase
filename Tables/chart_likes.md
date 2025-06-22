# Table: `chart_likes`

---

## Description

Tracks which users have liked which charts, enabling the system to monitor chart popularity and user engagement. Supports features like sorting charts by likes, personalized recommendations, and analytics on user behavior.

---

## Schema

| Column Name | Data Type | Null | Default             | Constraints | Description                             |
| ----------- | --------- | ---- | ------------------- | ----------- | --------------------------------------- |
| id          | UUID      | NO   | `gen_random_uuid()` | Primary Key | Unique identifier for the like record   |
| chart_id    | UUID      | YES  | NULL                | Foreign Key | References the chart that was liked     |
| user_id     | UUID      | YES  | NULL                | Foreign Key | References the user who liked the chart |
| liked_at    | TIMESTAMP | NO   | `CURRENT_TIMESTAMP` |             | Timestamp when the chart was liked      |

---

## Relationships

| Related Table | Relationship Type | Foreign Key | Description                        |
| ------------- | ----------------- | ----------- | ---------------------------------- |
| charts        | Many-to-One       | chart_id    | A chart can be liked by many users |
| users         | Many-to-One       | user_id     | A user can like many charts        |

---

## Example Record

```json
{
  "id": "ab85e0f0-9f1b-46e3-9a4a-7b9c834ca07d",
  "chart_id": "75bd2f4e-93c4-45c3-a249-91c1a18c926d",
  "user_id": "f4b7ef5e-2ef5-43cd-a739-e0de246472f5",
  "liked_at": "2025-06-21T15:35:00Z"
}
```

---

## Usage Scenarios

* Display most liked/popular charts on a dashboard.
* Recommend charts based on user like history.
* Analyze engagement trends over time using `liked_at`.
* Prevent duplicate likes by enforcing uniqueness per user and chart.

---

## Query Examples

### 1. Count likes per chart:

```sql
SELECT chart_id, COUNT(*) AS like_count
FROM chart_likes
GROUP BY chart_id
ORDER BY like_count DESC;
```

### 2. Check if a user liked a specific chart:

```sql
SELECT 1
FROM chart_likes
WHERE chart_id = '75bd2f4e-93c4-45c3-a249-91c1a18c926d'
  AND user_id = 'f4b7ef5e-2ef5-43cd-a739-e0de246472f5';
```

### 3. Get all charts liked by a user:

```sql
SELECT chart_id
FROM chart_likes
WHERE user_id = 'f4b7ef5e-2ef5-43cd-a739-e0de246472f5';
```

---

## Insert Example

```sql
INSERT INTO chart_likes (
  id, chart_id, user_id, liked_at
)
VALUES (
  gen_random_uuid(),
  '75bd2f4e-93c4-45c3-a249-91c1a18c926d',
  'f4b7ef5e-2ef5-43cd-a739-e0de246472f5',
  CURRENT_TIMESTAMP
)
ON CONFLICT (chart_id, user_id) DO NOTHING;
```
