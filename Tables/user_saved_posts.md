# Table: `user_saved_posts`

---

## Description

Keeps track of posts that users have saved for later reference. Also flags whether the user is the original author of the saved post.

---

## Schema

| Column Name | Data Type | Null | Default            | Constraints | Description                                              |
| ----------- | --------- | ---- | ------------------ | ----------- | -------------------------------------------------------- |
| `id`        | INT       | NO   | -                  | PRIMARY KEY | Unique identifier for the saved post record              |
| `user_id`   | UUID      | YES  | NULL               | FOREIGN KEY | References the user who saved the post                   |
| `post_id`   | UUID      | YES  | NULL               | FOREIGN KEY | References the post that was saved                       |
| `is_owner`  | BOOLEAN   | YES  | `false`            |             | Indicates if the user is the original author of the post |
| `saved_at`  | TIMESTAMP | NO   | CURRENT\_TIMESTAMP |             | Timestamp of when the post was saved                     |

---

## Relationships

| Related Table | Relationship Type | Foreign Key | Description                           |
| ------------- | ----------------- | ----------- | ------------------------------------- |
| `users`       | Many-to-One       | `user_id`   | A user can save multiple posts        |
| `posts`       | Many-to-One       | `post_id`   | A post can be saved by multiple users |

---

## Business Rules

* A user should not save the same post more than once. Consider enforcing a **unique constraint** on `(user_id, post_id)` to prevent duplicates.
* `is_owner = true` can be used to highlight the user's authorship (e.g., badge, permissions).
* `saved_at` can be used to sort posts by recency or activity.

---

## Indexes

| Index Name                     | Column    | Type  | Description                             |
| ------------------------------ | --------- | ----- | --------------------------------------- |
| `idx_user_saved_posts_user_id` | `user_id` | BTREE | Optimizes queries retrieving user saves |

---

## Example Row

```json
{
  "id": 121,
  "user_id": "bb13b8df-2f87-4f76-973e-123b5f7412ee",
  "post_id": "4a76f210-6c2e-4eac-b229-2dc6cdbe7153",
  "is_owner": true,
  "saved_at": "2025-06-21T10:20:00Z"
}
```

---

## Usage Scenarios

1. **Bookmarking**: Users can save posts they want to revisit later.
2. **Ownership Recognition**: Posts saved by their authors can be flagged for editorial control or visual distinction.
3. **Activity Tracking**: Useful for analytics around user engagement with content.

---

## Query Examples

### 1. Get all posts saved by a specific user:

```sql
SELECT post_id, is_owner, saved_at
FROM user_saved_posts
WHERE user_id = 'bb13b8df-2f87-4f76-973e-123b5f7412ee';
```

### 2. Find users who saved a specific post:

```sql
SELECT user_id, is_owner, saved_at
FROM user_saved_posts
WHERE post_id = '4a76f210-6c2e-4eac-b229-2dc6cdbe7153';
```

### 3. Fetch all posts where the user is also the owner:

```sql
SELECT post_id
FROM user_saved_posts
WHERE user_id = 'bb13b8df-2f87-4f76-973e-123b5f7412ee'
  AND is_owner = true;
```

---

## Insert Example

```sql
INSERT INTO user_saved_posts (
  id,
  user_id,
  post_id,
  is_owner,
  saved_at
) VALUES (
  121,
  'bb13b8df-2f87-4f76-973e-123b5f7412ee',
  '4a76f210-6c2e-4eac-b229-2dc6cdbe7153',
  true,
  NOW()
);
```

---