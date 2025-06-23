# Table: `user_preferences`

---

## Description

Stores configurable user-level settings that define the personalized experience for each user across sessions and platforms. This includes UI themes, language, timezone, notification preferences, and advanced layout options.

---

## Schema

| Column Name             | Data Type | Null | Default | Constraints | Description                                                        |
| ----------------------- | --------- | ---- | ------- | ----------- | ------------------------------------------------------------------ |
| `id`                    | UUID      | NO   | -       | Primary Key | Unique identifier for the preference record                        |
| `user_id`               | UUID      | YES  | -       | Foreign Key | References a user from the `users` table                           |
| `theme`                 | TEXT      | YES  | -       |             | User-selected UI theme (e.g., `"light"`, `"dark"`, or custom name) |
| `language`              | TEXT      | YES  | -       |             | Preferred language code (e.g., `"en"`, `"fr"`)                     |
| `brand_color`           | TEXT      | YES  | -       |             | Custom branding color (e.g., hex code like `"#FF5733"`)            |
| `timezone`              | TEXT      | YES  | -       |             | Preferred timezone (e.g., `"America/New_York"`)                    |
| `date_range`            | DATERANGE | YES  | -       |             | Preferred default date range for views and filters                 |
| `notifications_enabled` | BOOL      | YES  | `true`  |             | If true, user wants to receive notifications                       |
| `preferences_json`      | JSONB     | YES  | -       |             | Flexible object for storing additional structured preferences      |
| `created_at`            | TIMESTAMP | YES  | `now()` |             | Timestamp when the record was created                              |

---

## Relationships

| Related Table | Relationship Type | Foreign Key | Description                        |
| ------------- | ----------------- | ----------- | ---------------------------------- |
| `users`       | Many-to-One       | `user_id`   | Associates preferences with a user |

---

## Example Record

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

---

## Usage Scenarios

1. **Personalized UI Experience**: Load and apply user-specific theme, language, and layout settings on app login.
2. **Dashboard Filters Initialization**: Use the `date_range` and `timezone` for initializing filtered reports.
3. **Notification Toggle**: Respect the `notifications_enabled` flag before delivering any messages.
4. **Flexible Preference Extension**: Use `preferences_json` to evolve the preference schema without migrations.

---

## Query Examples

### 1. Get all preferences for a given user:

```sql
SELECT *
FROM user_preferences
WHERE user_id = '5a3c3c3e-ff00-4f17-bf27-527b4a73e774';
```

### 2. Fetch users who have disabled notifications:

```sql
SELECT user_id
FROM user_preferences
WHERE notifications_enabled = false;
```

### 3. Get dark theme users in a specific timezone:

```sql
SELECT user_id
FROM user_preferences
WHERE theme = 'dark' AND timezone = 'America/New_York';
```

### 4. Extract dashboard layout preference from JSON:

```sql
SELECT
  user_id,
  preferences_json->>'dashboard_layout' AS dashboard_layout
FROM user_preferences;
```

---

## Insert Example

```sql
INSERT INTO user_preferences (
  id,
  user_id,
  theme,
  language,
  brand_color,
  timezone,
  date_range,
  notifications_enabled,
  preferences_json,
  created_at
) VALUES (
  'aa8b92a0-99c7-41c3-9cd3-f5f8130e0e5b',
  '5a3c3c3e-ff00-4f17-bf27-527b4a73e774',
  'dark',
  'en',
  '#0044cc',
  'America/New_York',
  '[2025-01-01,2025-06-30]',
  true,
  '{"dashboard_layout": "compact", "show_tooltips": true}',
  now()
);
```

---
