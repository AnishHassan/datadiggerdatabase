# Table: `post`

# Description

Stores content created by users, including draft and published posts. Each post can contain a title and body content, and is linked to its author.

---

# Schema

| Column Name   | Data Type | Null | Default             | Constraints | Description                                    |
| ------------- | --------- | ---- | ------------------- | ----------- | ---------------------------------------------- |
| id            | UUID      | NO   | `gen_random_uuid()` | Primary Key | Unique identifier for the post                 |
| user_id       | UUID      | YES  | NULL                | Foreign Key | References the user who created the post       |
| title         | TEXT      | YES  | NULL                |             | Title of the post                              |
| content       | TEXT      | YES  | NULL                |             | Body content of the post                       |
| status        | TEXT      | YES  | `'draft'`           |             | Status of the post: e.g., `draft`, `published` |
| published_at  | TIMESTAMP | YES  | NULL                |             | Timestamp when the post was published          |
| created_at    | TIMESTAMP | NO   | CURRENT_TIMESTAMP   |             | Timestamp of when the post was created         |

---

# Relationships

| Related Table | Relationship Type | Foreign Key | Description                  |
| ------------- | ----------------- | ----------- | ---------------------------- |
| users         | Many-to-One       | user_id     | A user can create many posts |

---

# Business Rules

* Posts are created in `draft` status by default.
* `published_at` should be set only if the post is actually published.
* `title` and `content` should be validated to prevent empty submissions.
* `status` should be restricted to allowed values (`draft`, `published`, etc.).

---

# Indexes

| Index Name              | Column        | Type  | Description                                  |
| ----------------------- | ------------- | ----- | -------------------------------------------- |
| `idx_post_user_id`      | user_id       | BTREE | Speeds up queries by user                    |
| `idx_post_status`       | status        | BTREE | Optimizes filtering by post status           |
| `idx_post_published_at` | published_at  | BTREE | Useful for sorting and querying recent posts |

---

# Example Row

```json
{
  "id": "c3f8f529-4872-4412-b732-3b918b8e6e9f",
  "user_id": "f9e2d798-3c12-4cf2-a3b9-cf38e342fc10",
  "title": "Trum",
  "content": "Here are 5 actionable tips to stay focused and efficient...",
  "status": "published",
  "published_at": "2025-06-20T14:10:00Z",
  "created_at": "2025-06-19T09:32:00Z"
}
```