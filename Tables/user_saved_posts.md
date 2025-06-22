# Table: `user_saved_posts`

# Description
Keeps track of posts that users have saved for later reference. Also flags whether the user is the original author of the saved post.

---

# Schema

| Column Name | Data Type  | Null | Default           | Constraints       | Description                                                     |
|-------------|------------|------|-------------------|-------------------|-----------------------------------------------------------------|
| id          | INT        | NO   | -                 | Primary Key       | Unique identifier for the saved post record                    |
| user_id     | UUID       | YES  | NULL              | Foreign Key       | References the user who saved the post                         |
| post_id     | UUID       | YES  | NULL              | Foreign Key       | References the saved post                                      |
| is_owner    | BOOL       | YES  | `false`           |                   | Indicates if the user is the original author of the post       |
| saved_at    | TIMESTAMP  | NO   | CURRENT_TIMESTAMP |                   | Timestamp of when the post was saved                           |

---

# Relationships

| Related Table | Relationship Type | Foreign Key | Description                                       |
|---------------|-------------------|-------------|---------------------------------------------------|
| users         | Many-to-One       | user_id     | A user can save many posts                        |
| posts         | Many-to-One       | post_id     | A post can be saved by many users                 |

---

# Business Rules

- A user can only save the same post once — consider enforcing a unique constraint on `(user_id, post_id)` if needed.
- `is_owner = true` can help show authorship badges or unlock edit/delete rights.
- `saved_at` helps in sorting and timeline tracking for saved content.

---

# Indexes

| Index Name                     | Column    | Type   | Description                              |
|--------------------------------|-----------|--------|------------------------------------------|
| `idx_user_saved_posts_user_id` | user_id   | BTREE  | Improves query performance on user saves |

---

# Example Row

```json
{
  "id": 121,
  "user_id": "bb13b8df-2f87-4f76-973e-123b5f7412ee",
  "post_id": "4a76f210-6c2e-4eac-b229-2dc6cdbe7153",
  "is_owner": true,
  "saved_at": "2025-06-21T10:20:00Z"
}
