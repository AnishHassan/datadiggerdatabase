# Table: `user_saved_datasets`

# Description
Tracks datasets saved by users for quick access or reuse. Supports ownership flagging to distinguish between original authors and users who bookmarked datasets.

---

# Schema

| Column Name | Data Type  | Null | Default           | Constraints       | Description                                                        |
|-------------|------------|------|-------------------|-------------------|--------------------------------------------------------------------|
| id          | INT        | NO   | -                 | Primary Key       | Unique identifier for each saved dataset record                    |
| user_id     | UUID       | YES  | NULL              | Foreign Key       | References the user saving the dataset                             |
| dataset_id  | UUID       | YES  | NULL              | Foreign Key       | References the unique dataset being saved                          |
| is_owner    | BOOL       | YES  | `false`           |                   | Indicates whether the user is the original creator of the dataset  |
| saved_at    | TIMESTAMP  | NO   | CURRENT_TIMESTAMP |                   | Timestamp of when the dataset was saved                            |

---

# Relationships

| Related Table | Relationship Type | Foreign Key | Description                                       |
|---------------|-------------------|-------------|---------------------------------------------------|
| users         | Many-to-One       | user_id     | A user can save multiple datasets                 |
| datasets      | Many-to-One       | dataset_id  | A dataset can be saved by multiple users          |

---

# Business Rules

- Each dataset can be saved by multiple users, but only one should have `is_owner = true`.
- `is_owner` helps manage permissions, edit access, and ownership display logic.
- Duplicate saving of the same dataset by the same user can be prevented with a composite unique constraint if needed.
- `saved_at` is used to track when the dataset was bookmarked or created.

---

# Indexes

| Index Name                        | Column    | Type   | Description                               |
|----------------------------------|-----------|--------|-------------------------------------------|
| `idx_user_saved_datasets_user_id`| user_id   | BTREE  | Optimizes retrieval of datasets by user   |

---

# Example Row

```json
{
  "id": 302,
  "user_id": "df24109c-a95f-470e-9db6-fbd2d3ecb7e2",
  "dataset_id": "ba89f007-cf15-4ea9-88c3-49e2f93c93d3",
  "is_owner": false,
  "saved_at": "2025-06-21T11:35:00Z"
}
