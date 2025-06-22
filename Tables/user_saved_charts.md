#  Table: `user_saved_charts`

#  Description
Stores user-saved charts, allowing users to bookmark or reuse previously generated visual data insights. A chart may be saved by multiple users, and users can mark charts they created as owned.

---

# Schema

| Column Name | Data Type  | Null | Default           | Constraints       | Description                                                       |
|-------------|------------|------|--------------------|-------------------|-------------------------------------------------------------------|
| id          | INT        | NO   | -                  | Primary Key       | Unique identifier for each saved chart record                     |
| user_id     | UUID       | YES  | NULL               | Foreign Key       | References the user who saved the chart                           |
| chart_id    | UUID       | YES  | NULL               | Foreign Key       | References the unique chart that was saved                        |
| is_owner    | BOOL       | YES  | `false`            |                   | Indicates if the user is the creator (owner) of the chart         |
| saved_at    | TIMESTAMP  | NO   | CURRENT_TIMESTAMP  |                   | Timestamp of when the chart was saved                             |

---

# Relationships

| Related Table | Relationship Type | Foreign Key | Description                                      |
|---------------|-------------------|-------------|--------------------------------------------------|
| users         | Many-to-One       | user_id     | A user can save multiple charts                  |
| charts        | Many-to-One       | chart_id    | A chart can be saved by multiple users           |

---

# Business Rules

- A chart can be saved by multiple users, but only one user can be marked as the original creator via `is_owner = true`.
- Ownership (`is_owner`) helps with managing permissions, editing rights, and display prioritization.
- `saved_at` helps track when the user bookmarked or created the chart.

---

# Indexes

| Index Name                     | Column    | Type   | Description                                |
|--------------------------------|-----------|--------|--------------------------------------------|
| `idx_user_saved_charts_user_id`| user_id   | BTREE  | Speeds up user-based chart queries         |

---

# Example Row

```json
{
  "id": 201,
  "user_id": "d3e7c8f4-9b01-45e9-b55b-bbe8c5a9c91a",
  "chart_id": "a5b0192e-dbd2-49e0-9305-e3c44d4e2382",
  "is_owner": true,
  "saved_at": "2025-06-21T11:10:00Z"
}
