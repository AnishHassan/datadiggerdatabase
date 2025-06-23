# Table: `preferences`

---

## Description

The `preferences` table stores **user-specific customization settings**, allowing personalized experiences across the app. It supports static preferences (like theme and language) and dynamic configurations via a flexible `JSONB` field.

---

## Schema

| Column Name            | Data Type   | Null | Default         | Constraints | Description                                                |
| ---------------------- | ----------- | ---- | --------------- | ----------- | ---------------------------------------------------------- |
| `id`                   | INT4        | NO   | AUTO_INCREMENT  | Primary Key | Unique identifier for each preference record               |
| `theme`                | VARCHAR(50) | YES  | `NULL`          |             | Preferred UI theme (e.g., `'light'`, `'dark'`)             |
| `language`             | VARCHAR(50) | YES  | `NULL`          |             | Preferred app language (e.g., `'en'`, `'fr'`, `'es'`)      |
| `notificationsenabled` | BOOLEAN     | YES  | `true`          |             | Whether the user receives push/email notifications         |
| `settings`             | JSONB       | YES  | `NULL`          |             | Flexible key-value settings (e.g., layout, toggles, flags) |

---

## Relationships

| Related Table | Relationship Type | Foreign Key      | Description                                       |
| ------------- | ----------------- | ---------------- | ------------------------------------------------- |
| `users`       | One-to-One        | `preferences_id` | Each user can have one associated preferences row |

> Note: Relationship assumes `users.preferences_id` as the foreign key unless your schema inverts this.

---

## Business Rules

* A preferences row is created during or after **user onboarding**.
* `notificationsenabled` defaults to `true` and can be toggled.
* If `theme` or `language` is `NULL`, app should **fallback to system default** values.
* `settings` allows dynamic personalization such as toggles, display configurations, or custom flags.

---

## Suggested Indexes

| Index Name                 | Column                 | Type  | Description                                        |
| -------------------------- | ---------------------- | ----- | -------------------------------------------------- |
| `idx_preferences_theme`    | `theme`                | BTREE | Helps in filtering or analytics by theme           |
| `idx_preferences_language` | `language`             | BTREE | Useful for localization stats or user segmentation |
| `idx_preferences_notify`   | `notificationsenabled` | BTREE | Optimizes queries for notification toggling        |

---

## Example Record

```json
{
  "id": 11,
  "theme": "dark",
  "language": "en",
  "notificationsenabled": true,
  "settings": {
    "sidebarCollapsed": false,
    "showTooltips": true,
    "dashboardLayout": "grid"
  }
}
```

---

## Query Examples

### Get all users who have disabled notifications:

```sql
SELECT id
FROM preferences
WHERE notificationsenabled = false;
```

### Fetch users with dark theme using JSON settings:

```sql
SELECT id
FROM preferences
WHERE theme = 'dark'
  AND settings->>'sidebarCollapsed' = 'false';
```