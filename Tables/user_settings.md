# Table: `user_settings`

# Description
Stores individual user configuration preferences for privacy, security, and notifications. Each user has a single associated settings record that governs their account behavior and communication preferences.

---

# Schema

| Column Name           | Data Type | Null | Default           | Constraints | Description                                                                 |
|------------------------|-----------|------|-------------------|-------------|-----------------------------------------------------------------------------|
| user_id               | UUID      | NO   | -                 | Primary Key | The unique ID of the user (linked to the users table)                      |
| is_private            | BOOL      | YES  | FALSE             |             | Determines whether the user's profile is visible to others                 |
| email_notifications   | BOOL      | YES  | TRUE              |             | If true, the user will receive email updates/announcements                 |
| two_factor_enabled    | BOOL      | YES  | FALSE             |             | Indicates whether two-factor authentication is turned on                  |
| last_password_change  | TIMESTAMP | YES  | NULL              |             | The timestamp of the most recent password update for security tracking     |

---

# Relationships

| Related Table | Relationship Type | Foreign Key | Description                         |
|---------------|-------------------|-------------|-------------------------------------|
| users         | One-to-One        | user_id     | Each user has one `user_settings` row |

---

# Business Rules

- A `user_settings` record **must exist for each user** — create it at registration or on first access.
- The `user_id` serves as both the primary key and foreign key.
- This table helps manage **account visibility, security preferences, and communication options**.

---

# Security Considerations

- If `two_factor_enabled = true`, ensure your system requires a second factor (e.g., TOTP, SMS) during login.
- You may choose to automatically set `last_password_change` on password reset/update.

---

# Example Row

```json
{
  "user_id": "c7b58c1d-30fa-43e3-80e3-fc4140f6832d",
  "is_private": true,
  "email_notifications": true,
  "two_factor_enabled": false,
  "last_password_change": "2025-06-20T09:30:00Z"
}
