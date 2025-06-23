# Table: `user_saved_datasets`

---

## Description

Tracks datasets saved by users for quick access or reuse. Supports ownership flagging to differentiate between original authors and users who have merely bookmarked the dataset.

---

## Schema

| Column Name  | Data Type | Null | Default            | Constraints | Description                                                       |
| ------------ | --------- | ---- | ------------------ | ----------- | ----------------------------------------------------------------- |
| `id`         | INT       | NO   | -                  | PRIMARY KEY | Unique identifier for each saved dataset record                   |
| `user_id`    | UUID      | YES  | NULL               | FOREIGN KEY | References the user saving the dataset                            |
| `dataset_id` | UUID      | YES  | NULL               | FOREIGN KEY | References the unique dataset being saved                         |
| `is_owner`   | BOOLEAN   | YES  | `false`            |             | Indicates whether the user is the original creator of the dataset |
| `saved_at`   | TIMESTAMP | NO   | CURRENT\_TIMESTAMP |             | Timestamp of when the dataset was saved                           |

---

## Relationships

| Related Table | Relationship Type | Foreign Key  | Description                              |
| ------------- | ----------------- | ------------ | ---------------------------------------- |
| `users`       | Many-to-One       | `user_id`    | A user can save multiple datasets        |
| `datasets`    | Many-to-One       | `dataset_id` | A dataset can be saved by multiple users |

---

## Business Rules

* A dataset can be saved by multiple users, but only one should have `is_owner = true`.
* `is_owner = true` signifies original authorship and grants potential permissions for editing or deletion.
* Duplicate saving by the same user can optionally be prevented using a composite unique constraint (`user_id`, `dataset_id`).
* `saved_at` helps track dataset popularity and user activity over time.

---

## Indexes

| Index Name                        | Column    | Type  | Description                                |
| --------------------------------- | --------- | ----- | ------------------------------------------ |
| `idx_user_saved_datasets_user_id` | `user_id` | BTREE | Optimizes lookup of saved datasets by user |

---

## Example Row

```json
{
  "id": 302,
  "user_id": "df24109c-a95f-470e-9db6-fbd2d3ecb7e2",
  "dataset_id": "ba89f007-cf15-4ea9-88c3-49e2f93c93d3",
  "is_owner": false,
  "saved_at": "2025-06-21T11:35:00Z"
}
```

---

## Usage Scenarios

1. **Personal Library**: Users can bookmark datasets they find useful.
2. **Dataset Collaboration**: Multiple users can access the same dataset; one retains ownership.
3. **Audit History**: Use `saved_at` to monitor dataset access and reuse patterns.
4. **Ownership-Based Actions**: Restrict edits or deletion to dataset owners.

---

## Query Examples

### 1. Get all datasets saved by a specific user:

```sql
SELECT dataset_id, is_owner, saved_at
FROM user_saved_datasets
WHERE user_id = 'df24109c-a95f-470e-9db6-fbd2d3ecb7e2';
```

### 2. Get all users who saved a particular dataset:

```sql
SELECT user_id, is_owner, saved_at
FROM user_saved_datasets
WHERE dataset_id = 'ba89f007-cf15-4ea9-88c3-49e2f93c93d3';
```

### 3. Find all datasets a user owns:

```sql
SELECT dataset_id
FROM user_saved_datasets
WHERE user_id = 'df24109c-a95f-470e-9db6-fbd2d3ecb7e2'
  AND is_owner = true;
```

---

## Insert Example

```sql
INSERT INTO user_saved_datasets (
  id,
  user_id,
  dataset_id,
  is_owner,
  saved_at
) VALUES (
  302,
  'df24109c-a95f-470e-9db6-fbd2d3ecb7e2',
  'ba89f007-cf15-4ea9-88c3-49e2f93c93d3',
  false,
  NOW()
);
```

---