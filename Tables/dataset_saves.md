# Table: `dataset_saves`

# Description

Tracks which datasets users have saved/bookmarked for later use.

---

# Schema

| Column Name | Data Type | Null | Default             | Constraints | Description                               |
| ----------- | --------- | ---- | ------------------- | ----------- | ----------------------------------------- |
| id          | UUID      | NO   | `gen_random_uuid()` | Primary Key | Unique identifier for the save record     |
| user_id     | UUID      | YES  | NULL                | Foreign Key | References the user who saved the dataset |
| dataset_id  | UUID      | YES  | NULL                | Foreign Key | References the dataset that was saved     |
| saved_at    | TIMESTAMP | NO   | `CURRENT_TIMESTAMP` |             | Timestamp when the dataset was saved      |

---

# Relationships

| Related Table | Relationship Type | Foreign Key | Description                          |
| ------------- | ----------------- | ----------- | ------------------------------------ |
| users         | Many-to-One       | user_id     | A user can save many datasets        |
| datasets      | Many-to-One       | dataset_id  | A dataset can be saved by many users |

---

# Business Rules

* A user should not be able to save the same dataset more than once — enforced via a unique index on `(user_id, dataset_id)`.
* Saved datasets may appear in a user's dashboard or bookmarks section.
* `saved_at` can be used to sort saved datasets chronologically.

---

# Indexes

| Index Name                             | Column(s)               | Type  | Description                                  |
| -------------------------------------- | ----------------------- | ----- | -------------------------------------------- |
| `dataset_saves_user_id_dataset_id_key` | (user_id, dataset_id)   | BTREE | Ensures uniqueness and improves lookup speed |

---

# Example Row

```json
{
  "id": "e1d3ffea-7a18-48b0-94be-5c8b1f3c037e",
  "user_id": "bb13b8df-2f87-4f76-973e-123b5f7412ee",
  "dataset_id": "6cce97e6-3fc1-4a87-b327-95fbd82c47d7",
  "saved_at": "2025-06-21T14:45:00Z"
}
```