# Table: `user_preferences`

## Description

Stores customizable preferences for individual users, such as UI theme, language, timezone, and notification settings. This allows for a personalized user experience across sessions and platforms.

---

## Schema

| Column Name            | Data Type | Null | Default | Constraints | Description                                                        |
| ---------------------- | --------- | ---- | ------- | ----------- | ------------------------------------------------------------------ |
| id                     | UUID      | NO   | -       | Primary Key | Unique identifier for the preference record                        |
| user_id                | UUID      | YES  | -       | Foreign Key | References a user (typically from a `users` table)                 |
| theme                  | TEXT      | YES  | -       |             | Selected UI theme (e.g., "light", "dark", or custom name)          |
| language               | TEXT      | YES  | -       |             | Preferred language code (e.g., "en", "fr")                         |
| brand_color            | TEXT      | YES  | -       |             | Custom brand color selection (e.g., hex code "#FF5733")            |
| timezone               | TEXT      | YES  | -       |             | User's preferred timezone (e.g., "America/New\_York")              |
| date_range             | DATERANGE | YES  | -       |             | Preferred default date range selection for views/filters           |
| notifications_enabled  | BOOL      | YES  | `true`  |             | Whether user wants to receive notifications                        |
| preferences_json       | JSONB     | YES  | -       |             | Flexible JSON object for storing additional structured preferences |
| created_at             | TIMESTAMP | YES  | `now()` |             | Timestamp when the preference was initially created                |

---

## Relationships

| Related Table | Relationship Type | Foreign Key | Description                        |
| ------------- | ----------------- | ----------- | ---------------------------------- |
| `users`       | Many-to-One       | `user_id`   | Associates preferences with a user |

---

## Business Rules

* Each user should have at most one `user_preferences` record.
* `preferences_json` may store advanced settings (e.g., layout, filters).
* `date_range` can help initialize default date filters in reports or dashboards.

---

## Indexes

| Index Name              | Column   | Type  | Description                                            |
| ----------------------- | -------- | ----- | ------------------------------------------------------ |
| `user_preferences_pkey` | id       | BTREE | Primary key for identifying individual preference rows |
| (optional)              | user_id  | BTREE | Recommended for frequent lookups by `user_id`          |

---

## Example Row

```json
{
  "id": "aa8b92a0-99c7-41c3-9cd3-f5f8130e0e5b",
  "user_id": "5a3c3c3e-ff00-4f17-bf27-527b4a73e774",
  "theme": "dark",
  "language": "en",
  "brand_color": "#0044cc",
  "timezone": "America/New_York",
  "date_range": "[2025-01-01,2025-06-30]",
  "notifications_enabled": true,
  "preferences_json": {
    "dashboard_layout": "compact",
    "show_tooltips": true
  },
  "created_at": "2025-06-21T09:12:33.215Z"
}
```