# Table: `dataset_saves`

---

# Description

Tracks which datasets users have saved or bookmarked for quick access or later reference. Helps power features like dashboards, favorites, and personalized dataset views.

---

# Schema

| Column Name  | Data Type | Null | Default             | Constraints | Description                                      |
| ------------ | --------- | ---- | ------------------- | ----------- | ------------------------------------------------ |
| `id`         | UUID      | NO   | `gen_random_uuid()` | Primary Key | Unique identifier for the save record            |
| `user_id`    | UUID      | YES  | NULL                | Foreign Key | References the user who saved the dataset        |
| `dataset_id` | UUID      | YES  | NULL                | Foreign Key | References the dataset that was saved            |
| `saved_at`   | TIMESTAMP | NO   | `CURRENT_TIMESTAMP` |             | Timestamp when the dataset was saved by the user |

---

# Relationships

| Related Table | Relationship Type | Foreign Key  | Description                                     |
| ------------- | ----------------- | ------------ | ----------------------------------------------- |
| `users`       | Many-to-One       | `user_id`    | A user can save/bookmark many datasets          |
| `datasets`    | Many-to-One       | `dataset_id` | A dataset can be saved/bookmarked by many users |

---

# Example Record

```json
{
  "id": "e1d3ffea-7a18-48b0-94be-5c8b1f3c037e",
  "user_id": "bb13b8df-2f87-4f76-973e-123b5f7412ee",
  "dataset_id": "6cce97e6-3fc1-4a87-b327-95fbd82c47d7",
  "saved_at": "2025-06-21T14:45:00Z"
}
```

---

# Usage Scenarios

* Showing a user’s bookmarked datasets on their dashboard.
* Preventing duplicate saves using the `(user_id, dataset_id)` uniqueness constraint.
* Allowing users to manage or reorder saved datasets by `saved_at`.
* Supporting features like "Recently Saved" or "Favorites".

---

# Query Examples

# 1. Get all datasets saved by a specific user

```sql
SELECT dataset_id, saved_at
FROM dataset_saves
WHERE user_id = 'bb13b8df-2f87-4f76-973e-123b5f7412ee'
ORDER BY saved_at DESC;
```

# 2. Check if a dataset is already saved by a user

```sql
SELECT 1
FROM dataset_saves
WHERE user_id = 'bb13b8df-2f87-4f76-973e-123b5f7412ee'
  AND dataset_id = '6cce97e6-3fc1-4a87-b327-95fbd82c47d7';
```

# 3. Count how many users have saved a given dataset

```sql
SELECT COUNT(*) AS total_saves
FROM dataset_saves
WHERE dataset_id = '6cce97e6-3fc1-4a87-b327-95fbd82c47d7';
```

---

# Insert Example

```sql
INSERT INTO dataset_saves (
  id,
  user_id,
  dataset_id,
  saved_at
) VALUES (
  gen_random_uuid(),
  'bb13b8df-2f87-4f76-973e-123b5f7412ee',
  '6cce97e6-3fc1-4a87-b327-95fbd82c47d7',
  CURRENT_TIMESTAMP
);
```