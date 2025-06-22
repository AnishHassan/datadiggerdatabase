# Table: `feature_usage`

---

## **Description**

Tracks how frequently individual users interact with specific features within the platform. Enables detailed behavioral analytics, adoption tracking, and optimization of user-facing functionalities.

---

## **Schema**

| Column Name    | Data Type    | Null | Default             | Constraints | Description                                                   |
| -------------- | ------------ | ---- | ------------------- | ----------- | ------------------------------------------------------------- |
| `id`           | INT          | NO   | –                   | Primary Key | Unique identifier for each feature usage record               |
| `user_id`      | UUID         | YES  | NULL                | Foreign Key | References the user engaging with the feature                 |
| `feature_name` | VARCHAR(100) | YES  | NULL                | –           | Name or identifier of the feature (e.g., `"calendar_follow"`) |
| `metadata`     | JSONB        | YES  | NULL                | –           | Flexible JSON object for storing contextual or dynamic data   |
| `usage_count`  | INT          | YES  | `1`                 | –           | Count of how many times the feature has been used by the user |
| `last_used`    | TIMESTAMP    | NO   | `CURRENT_TIMESTAMP` | –           | Timestamp of the most recent usage                            |

---

## **Relationships**

| Related Table | Relationship Type | Foreign Key | Description                                         |
| ------------- | ----------------- | ----------- | --------------------------------------------------- |
| `users`       | Many-to-One       | `user_id`   | A user can have multiple interactions with features |

---

## **Example Record**

```json
{
  "id": 578,
  "user_id": "22c9d63f-f1a3-4879-b1f3-1a1d5ed89f01",
  "feature_name": "calendar_follow_event",
  "metadata": {
    "event_id": "7894d5ac-a3f2-4a81-944b-914dba2f413f"
  },
  "usage_count": 4,
  "last_used": "2025-06-21T13:15:00Z"
}
```

---

## **Usage Scenarios**

* Tracking feature adoption per user or globally.
* Identifying underused features or potential UX issues.
* Displaying most-used features on user dashboards or admin panels.
* Supporting usage-based recommendations or A/B testing.
* Auditing feature access in high-security contexts.
* Detecting power users or usage anomalies (e.g., spikes).

---

## **Query Examples**

### Get top 5 most-used features across all users

```sql
SELECT feature_name, SUM(usage_count) AS total_usage
FROM feature_usage
GROUP BY feature_name
ORDER BY total_usage DESC
LIMIT 5;
```

---

### Find features a specific user has interacted with in the past week

```sql
SELECT *
FROM feature_usage
WHERE user_id = '22c9d63f-f1a3-4879-b1f3-1a1d5ed89f01'
  AND last_used >= CURRENT_DATE - INTERVAL '7 days';
```

---

### Count of users per feature (adoption metric)

```sql
SELECT feature_name, COUNT(DISTINCT user_id) AS unique_users
FROM feature_usage
GROUP BY feature_name
ORDER BY unique_users DESC;
```

---

## **Insert Example**

```sql
INSERT INTO feature_usage (user_id, feature_name, metadata, usage_count, last_used)
VALUES (
  '22c9d63f-f1a3-4879-b1f3-1a1d5ed89f01',
  'calendar_follow_event',
  '{"event_id": "7894d5ac-a3f2-4a81-944b-914dba2f413f"}',
  1,
  CURRENT_TIMESTAMP
);
```