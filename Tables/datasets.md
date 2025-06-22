# Table: `datasets`

# Description

Stores metadata about uploaded datasets, including visibility settings, schema definitions, and usage statistics.

---

# Schema

| Column Name             | Data Type | Null | Default             | Constraints | Description                                                       |
| ----------------------- | --------- | ---- | ------------------- | ----------- | ----------------------------------------------------------------- |
| id                      | UUID      | NO   | `gen_random_uuid()` | Primary Key | Unique identifier for the dataset                                 |
| title                   | TEXT      | YES  | NULL                |             | Title of the dataset                                              |
| description             | TEXT      | YES  | NULL                |             | A brief description of the dataset                                |
| schema                  | JSONB     | YES  | NULL                |             | JSON structure representing the dataset schema                    |
| file_url                | TEXT      | YES  | NULL                |             | URL or path where the dataset file is stored                      |
| is_public               | BOOL      | YES  | `false`             |             | Indicates if the dataset is visible to all users                  |
| is_admin_created        | BOOL      | YES  | `false`             |             | Flag to identify if the dataset was uploaded by an admin          |
| created_by              | UUID      | YES  | NULL                | Foreign Key | References the user who uploaded the dataset                      |
| created_at              | TIMESTAMP | NO   | `CURRENT_TIMESTAMP` |             | Timestamp of when the dataset was created                         |
| updated_at              | TIMESTAMP | NO   | `CURRENT_TIMESTAMP` |             | Timestamp of the most recent update to the dataset                |
| used_in_charts_count    | INT4      | YES  | 0                   |             | Tracks how many charts currently use this dataset                 |
| version                 | INT4      | YES  | 1                   |             | Version number of the dataset, useful for managing schema changes |

---

# Relationships

| Related Table | Relationship Type | Foreign Key | Description                     |
| ------------- | ----------------- | ----------- | ------------------------------- |
| users         | Many-to-One       | created_by  | A user can create many datasets |

---

# Business Rules

* `is_public = true` allows dataset discovery by all users.
* `is_admin_created = true` is used to distinguish between curated datasets and user-uploaded ones.
* Dataset `schema` can be used to validate or preview the structure before usage.
* `used_in_charts_count` helps track impact and popularity of datasets.


# Example Row

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