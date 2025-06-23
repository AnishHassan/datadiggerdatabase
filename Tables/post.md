# Table: `post`

---

## Description

Represents **user-generated content**, including both **drafts** and **published posts**.
Each post consists of a `title`, `content`, and optional `published_at` timestamp, and is associated with the **user** who authored it.

---

## Schema

| Column Name    | Data Type | Null | Default             | Constraints | Description                                       |
| -------------- | --------- | ---- | ------------------- | ----------- | ------------------------------------------------- |
| `id`           | UUID      | NO   | `gen_random_uuid()` | Primary Key | Unique identifier for each post                   |
| `user_id`      | UUID      | YES  | `NULL`              | Foreign Key | Refers to the authoring user (`users.id`)         |
| `title`        | TEXT      | YES  | `NULL`              |             | Title of the post                                 |
| `content`      | TEXT      | YES  | `NULL`              |             | Main body content                                 |
| `status`       | TEXT      | YES  | `'draft'`           |             | Status of the post (`draft`, `published`, etc.)   |
| `published_at` | TIMESTAMP | YES  | `NULL`              |             | Set when the post is published; `NULL` for drafts |
| `created_at`   | TIMESTAMP | NO   | `CURRENT_TIMESTAMP` |             | Timestamp when the post was created               |

---

## Relationships

| Related Table | Relationship Type | Foreign Key | Description                     |
| ------------- | ----------------- | ----------- | ------------------------------- |
| `users`       | Many-to-One       | `user_id`   | Each user can author many posts |

---

## Business Rules

* Posts **default to `draft`** until explicitly published.
* `published_at` **must not be set** unless `status = 'published'`.
* Empty `title` or `content` should be disallowed via application-level validation.
* `status` should be **restricted to an enum or predefined list** (`draft`, `published`, optionally `archived`, `scheduled`, etc.).

---

## Indexes

| Index Name              | Column         | Type  | Description                                     |
| ----------------------- | -------------- | ----- | ----------------------------------------------- |
| `idx_post_user_id`      | `user_id`      | BTREE | Speeds up user-specific content queries         |
| `idx_post_status`       | `status`       | BTREE | Enables efficient filtering by post state       |
| `idx_post_published_at` | `published_at` | BTREE | Useful for feed ordering, scheduling, analytics |

---

## Example Record

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

---

## Query Examples

### Fetch all published posts

```sql
SELECT id, title, published_at
FROM post
WHERE status = 'published'
ORDER BY published_at DESC;
```

### Get latest drafts by a specific user

```sql
SELECT title, created_at
FROM post
WHERE user_id = 'f9e2d798-3c12-4cf2-a3b9-cf38e342fc10'
  AND status = 'draft'
ORDER BY created_at DESC;
```

### Count published posts per user

```sql
SELECT user_id, COUNT(*) AS post_count
FROM post
WHERE status = 'published'
GROUP BY user_id;
```