# Table: `datasets`

---

# Description

Stores metadata and configuration about uploaded datasets, including schema structure, visibility settings, source information, and usage analytics. These datasets serve as the foundation for charting, analysis, and collaborative data work.

---

# Schema

| Column Name            | Data Type | Null | Default             | Constraints | Description                                                         |
| ---------------------- | --------- | ---- | ------------------- | ----------- | ------------------------------------------------------------------- |
| `id`                   | UUID      | NO   | `gen_random_uuid()` | Primary Key | Unique identifier for the dataset                                   |
| `title`                | TEXT      | YES  | NULL                |             | Title of the dataset                                                |
| `description`          | TEXT      | YES  | NULL                |             | A brief textual description of the dataset                          |
| `schema`               | JSONB     | YES  | NULL                |             | JSON structure defining the dataset schema                          |
| `file_url`             | TEXT      | YES  | NULL                |             | URL or file path where the dataset is stored                        |
| `is_public`            | BOOL      | YES  | `false`             |             | Whether the dataset is publicly accessible                          |
| `is_admin_created`     | BOOL      | YES  | `false`             |             | Whether the dataset was created/uploaded by an admin                |
| `created_by`           | UUID      | YES  | NULL                | Foreign Key | References the user who uploaded the dataset                        |
| `created_at`           | TIMESTAMP | NO   | `CURRENT_TIMESTAMP` |             | Timestamp of dataset creation                                       |
| `updated_at`           | TIMESTAMP | NO   | `CURRENT_TIMESTAMP` |             | Timestamp of the most recent update to the dataset                  |
| `used_in_charts_count` | INT4      | YES  | `0`                 |             | Number of charts currently using this dataset                       |
| `version`              | INT4      | YES  | `1`                 |             | Version number of the dataset, useful for schema or content updates |

---

# Relationships

| Related Table | Relationship Type | Foreign Key  | Description                         |
| ------------- | ----------------- | ------------ | ----------------------------------- |
| `users`       | Many-to-One       | `created_by` | A user can upload multiple datasets |

---

# Example Record

```json
{
  "id": "6cce97e6-3fc1-4a87-b327-95fbd82c47d7",
  "title": "Global Temperature Records",
  "description": "Historical temperature data across major regions",
  "schema": {
    "fields": [
      { "name": "year", "type": "integer" },
      { "name": "region", "type": "string" },
      { "name": "temperature", "type": "float" }
    ]
  },
  "file_url": "https://cdn.example.com/datasets/temp_data_v1.csv",
  "is_public": true,
  "is_admin_created": false,
  "created_by": "dcb9f3b2-e6aa-4622-a517-b1fead6b4761",
  "created_at": "2025-06-21T13:00:00Z",
  "updated_at": "2025-06-21T13:00:00Z",
  "used_in_charts_count": 4,
  "version": 1
}
```

---

# Usage Scenarios

* Displaying public datasets for all users to explore and chart.
* Showing a user’s uploaded datasets in their profile/dashboard.
* Managing schema evolution via `version`.
* Tracking dataset popularity or reuse with `used_in_charts_count`.
* Filtering admin-curated datasets using `is_admin_created`.

---

# Query Examples

# 1. Fetch all public datasets with usage count

```sql
SELECT title, used_in_charts_count
FROM datasets
WHERE is_public = true
ORDER BY used_in_charts_count DESC;
```

# 2. Get all datasets uploaded by a specific user

```sql
SELECT id, title, created_at
FROM datasets
WHERE created_by = 'dcb9f3b2-e6aa-4622-a517-b1fead6b4761'
ORDER BY created_at DESC;
```

# 3. Retrieve dataset schema for validation

```sql
SELECT schema
FROM datasets
WHERE id = '6cce97e6-3fc1-4a87-b327-95fbd82c47d7';
```

---

# Insert Example

```sql
INSERT INTO datasets (
  id,
  title,
  description,
  schema,
  file_url,
  is_public,
  is_admin_created,
  created_by,
  created_at,
  updated_at,
  used_in_charts_count,
  version
) VALUES (
  gen_random_uuid(),
  'Global Temperature Records',
  'Historical temperature data across major regions',
  '{
    "fields": [
      { "name": "year", "type": "integer" },
      { "name": "region", "type": "string" },
      { "name": "temperature", "type": "float" }
    ]
  }'::jsonb,
  'https://cdn.example.com/datasets/temp_data_v1.csv',
  true,
  false,
  'dcb9f3b2-e6aa-4622-a517-b1fead6b4761',
  CURRENT_TIMESTAMP,
  CURRENT_TIMESTAMP,
  0,
  1
);
```