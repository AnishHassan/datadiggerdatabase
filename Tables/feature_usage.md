# Table: `feature_usage`

# Description

Tracks how frequently users interact with specific features within the platform. Useful for analytics and feature optimization.

---

# Schema

| Column Name   | Data Type    | Null | Default            | Constraints | Description                                      |
| ------------- | ------------ | ---- | ------------------ | ----------- | ------------------------------------------------ |
| id            | INT          | NO   | -                  | Primary Key | Unique identifier for the feature usage record   |
| user_id       | UUID         | YES  | NULL               | Foreign Key | References the user using the feature            |
| feature_name  | VARCHAR(100) | YES  | NULL               |             | Name of the feature being tracked                |
| metadata      | JSONB        | YES  | NULL               |             | Additional data/context related to feature usage |
| usage_count   | INT          | YES  | `1`                |             | Number of times the user has used the feature    |
| last_used     | TIMESTAMP    | NO   | CURRENT_TIMESTAMP  |             | Timestamp of the most recent use of the feature  |

---

# Relationships

| Related Table | Relationship Type | Foreign Key | Description                            |
| ------------- | ----------------- | ----------- | -------------------------------------- |
| users         | Many-to-One       | user_id     | A user can interact with many features |

---

# Business Rules

* `usage_count` is incremented every time the user uses the feature.
* `last_used` helps determine recent activity for individual features.
* Useful for monitoring adoption of new or experimental features.
* Consider creating a unique index on `(user_id, feature_name)` to avoid duplicate rows.

---

# Indexes

| Index Name                  | Column        | Type  | Description                                   |
| --------------------------- | ------------- | ----- | --------------------------------------------- |
| `idx_feature_usage_user_id` | user_id       | BTREE | Speeds up user-specific feature usage queries |
| `idx_feature_usage_name`    | feature_name  | BTREE | Helps filter or sort by feature               |

---

# Example Row

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

