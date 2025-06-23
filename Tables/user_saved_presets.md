# Table: `user_saved_presets`

---

## Description

Stores reusable sets of user-defined preferences—called **presets**—to customize the application experience, such as filters, UI themes, locales, and date-based settings.

---

## Schema

| Column Name   | Data Type | Null | Default | Constraints | Description                                                               |
| ------------- | --------- | ---- | ------- | ----------- | ------------------------------------------------------------------------- |
| `id`          | UUID      | NO   | -       | PRIMARY KEY | Unique identifier for the saved preset                                    |
| `user_id`     | UUID      | YES  | -       | FOREIGN KEY | References the user who owns the preset                                   |
| `name`        | TEXT      | NO   | -       |             | User-assigned name for the preset                                         |
| `theme`       | TEXT      | YES  | -       |             | UI theme preference (e.g., `light`, `dark`)                               |
| `language`    | TEXT      | YES  | -       |             | Preferred language setting                                                |
| `brand_color` | TEXT      | YES  | -       |             | Optional brand color for UI personalization                               |
| `regions`     | TEXT[]    | YES  | -       |             | List of regions the preset applies to                                     |
| `industries`  | TEXT[]    | YES  | -       |             | List of industries the preset filters by                                  |
| `categories`  | TEXT[]    | YES  | -       |             | Relevant content categories for this preset                               |
| `filters`     | JSONB     | YES  | -       |             | Flexible object storing custom filter data (e.g., tags, score thresholds) |
| `date_range`  | DATERANGE | YES  | -       |             | Date range the preset applies to                                          |
| `timezone`    | TEXT      | YES  | -       |             | Timezone context for scheduling or filtering                              |
| `created_at`  | TIMESTAMP | YES  | `now()` |             | Timestamp indicating when the preset was created                          |

---

## Relationships

| Related Table | Relationship Type | Foreign Key | Description                              |
| ------------- | ----------------- | ----------- | ---------------------------------------- |
| `users`       | Many-to-One       | `user_id`   | A user can create multiple saved presets |

---

## Suggested Indexes

| Index Name                      | Columns           | Type  | Description                                         |
| ------------------------------- | ----------------- | ----- | --------------------------------------------------- |
| `user_saved_presets_pkey`       | `id`              | BTREE | Uniquely identifies each preset                     |
| `user_saved_presets_userid_idx` | `user_id`         | BTREE | Speeds up lookup of presets by user                 |
| `user_saved_presets_name_idx`   | `user_id`, `name` | BTREE | Can be used to enforce unique preset names per user |

> **Note**: Consider adding a **composite unique constraint** on `(user_id, name)` to prevent duplicate preset names per user.

---

## Business Logic Highlights

* A user can store **multiple named presets** for different use cases (e.g., morning view, industry-specific filters).
* `filters` column uses `JSONB` to allow **schema-less extension** for advanced filter types.
* `date_range` and `timezone` allow **time-sensitive** and **locale-aware** personalization.

---

## Example Row

```json
{
  "id": "a567cb12-3abc-4567-9f89-9287d12a6fd4",
  "user_id": "e89b2e3d-9cde-45d1-812a-1f2d9330aa21",
  "name": "Default Preset",
  "theme": "dark",
  "language": "en",
  "brand_color": "#ff6600",
  "regions": ["US", "EU"],
  "industries": ["Tech", "Finance"],
  "categories": ["AI", "Startups"],
  "filters": {
    "min_score": 80,
    "source": "trusted"
  },
  "date_range": "[2024-01-01,2024-12-31]",
  "timezone": "UTC",
  "created_at": "2025-06-21T10:34:00Z"
}
```

---

## Query Examples

### 1. Get all presets created by a specific user:

```sql
SELECT * FROM user_saved_presets
WHERE user_id = 'e89b2e3d-9cde-45d1-812a-1f2d9330aa21';
```

### 2. Check if a preset name already exists for a user:

```sql
SELECT 1 FROM user_saved_presets
WHERE user_id = 'e89b2e3d-9cde-45d1-812a-1f2d9330aa21'
  AND name = 'Default Preset';
```

### 3. Filter presets using JSONB filters (e.g., `min_score >= 80`):

```sql
SELECT * FROM user_saved_presets
WHERE filters ->> 'min_score' >= '80';
```

---