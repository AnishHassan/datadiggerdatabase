Here’s your completed and optimized documentation for the `user_saved_charts` table, with everything standardized and the missing sections filled in:

---

# Table: `user_saved_charts`

---

## Description

Stores user-saved charts, enabling users to bookmark or reuse previously generated visual insights. A chart may be saved by multiple users, and users can indicate ownership for management and permissions.

---

## Schema

| Column Name | Data Type | Null | Default            | Constraints | Description                                               |
| ----------- | --------- | ---- | ------------------ | ----------- | --------------------------------------------------------- |
| `id`        | INT       | NO   | -                  | PRIMARY KEY | Unique identifier for each saved chart record             |
| `user_id`   | UUID      | YES  | NULL               | FOREIGN KEY | References the user who saved the chart                   |
| `chart_id`  | UUID      | YES  | NULL               | FOREIGN KEY | References the unique chart that was saved                |
| `is_owner`  | BOOLEAN   | YES  | `false`            |             | Indicates if the user is the creator (owner) of the chart |
| `saved_at`  | TIMESTAMP | NO   | CURRENT\_TIMESTAMP |             | Timestamp of when the chart was saved                     |

---

## Relationships

| Related Table | Relationship Type | Foreign Key | Description                            |
| ------------- | ----------------- | ----------- | -------------------------------------- |
| `users`       | Many-to-One       | `user_id`   | A user can save multiple charts        |
| `charts`      | Many-to-One       | `chart_id`  | A chart can be saved by multiple users |

---

## Business Rules

* A chart can be saved by multiple users, but only one user can be marked as the creator using `is_owner = true`.
* Charts with `is_owner = true` grant the user edit rights and display priority.
* `saved_at` is used to track when the chart was saved/bookmarked.

---

## Indexes

| Index Name                      | Column    | Type  | Description                               |
| ------------------------------- | --------- | ----- | ----------------------------------------- |
| `idx_user_saved_charts_user_id` | `user_id` | BTREE | Optimizes lookups of saved charts by user |

---

## Example Row

```json
{
  "id": 201,
  "user_id": "d3e7c8f4-9b01-45e9-b55b-bbe8c5a9c91a",
  "chart_id": "a5b0192e-dbd2-49e0-9305-e3c44d4e2382",
  "is_owner": true,
  "saved_at": "2025-06-21T11:10:00Z"
}
```

---

## Usage Scenarios

1. **Bookmarking**: Users bookmark frequently used charts for easy access.
2. **Shared Dashboards**: Teams view shared charts with clearly defined ownership.
3. **Version Control**: Distinguish between reused charts and original authored versions.
4. **Audit & Tracking**: Use `saved_at` to analyze chart popularity over time.

---

## Query Examples

### 1. Get all charts saved by a specific user:

```sql
SELECT chart_id, is_owner, saved_at
FROM user_saved_charts
WHERE user_id = 'd3e7c8f4-9b01-45e9-b55b-bbe8c5a9c91a';
```

### 2. Get all users who saved a specific chart:

```sql
SELECT user_id, is_owner, saved_at
FROM user_saved_charts
WHERE chart_id = 'a5b0192e-dbd2-49e0-9305-e3c44d4e2382';
```

### 3. Get all charts a user created (owns):

```sql
SELECT chart_id
FROM user_saved_charts
WHERE user_id = 'd3e7c8f4-9b01-45e9-b55b-bbe8c5a9c91a'
  AND is_owner = true;
```

---

## Insert Example

```sql
INSERT INTO user_saved_charts (
  id,
  user_id,
  chart_id,
  is_owner,
  saved_at
) VALUES (
  201,
  'd3e7c8f4-9b01-45e9-b55b-bbe8c5a9c91a',
  'a5b0192e-dbd2-49e0-9305-e3c44d4e2382',
  true,
  NOW()
);
```
