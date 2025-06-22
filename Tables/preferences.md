# Table: `preferences`

# Description
Stores customizable user-specific settings to enhance personalization, including display themes, language preferences, notification settings, and advanced configuration stored in JSON format.

---

# Schema

| Column Name         | Data Type     | Null | Default     | Constraints        | Description                                              |
|---------------------|---------------|------|-------------|---------------------|----------------------------------------------------------|
| id                  | INT4          | NO   | AUTO_INCREMENT | PRIMARY KEY     | Unique identifier for the preferences record             |
| theme               | VARCHAR(50)   | YES  | NULL        |                     | Preferred theme (e.g., `dark`, `light`)                  |
| language            | VARCHAR(50)   | YES  | NULL        |                     | Preferred UI language (e.g., `en`, `fr`, `es`)           |
| notificationsenabled| BOOLEAN       | YES  | `true`      |                     | Enable/disable push or email notifications               |
| settings            | JSONB         | YES  | NULL        |                     | Additional flexible settings in JSON format              |

---

# Relationships

| Related Table   | Relationship Type | Foreign Key     | Description                              |
|-----------------|-------------------|------------------|------------------------------------------|
| users           | One-to-One        | `preferences_id` | Linked to each user for personalization  |

---

# Business Rules

- Preferences are created during or after user onboarding.
- The `settings` field stores dynamic configurations (e.g., dashboard layout).
- `notificationsenabled` is `true` by default and can be toggled by the user.
- `theme` and `language` values are optional and fallback to system defaults if null.

---

# Example Row

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
