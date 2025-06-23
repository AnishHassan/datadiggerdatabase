# Table: `user_settings`

---

### Description

Stores individual configuration preferences for each user related to privacy, communication, and account security. This table enables personalized behavior such as two-factor authentication, email notification toggles, and profile visibility settings. Each user has exactly one corresponding settings record.

---

### Schema

| Column Name            | Data Type | Null | Default | Constraints              | Description                                                           |
| ---------------------- | --------- | ---- | ------- | ------------------------ | --------------------------------------------------------------------- |
| user_id                | UUID      | NO   | -       | Primary Key, Foreign Key | Unique identifier for the user (linked to the `users` table)          |
| is_private             | BOOLEAN   | YES  | FALSE   |                          | Controls whether the user's profile is visible to others              |
| email_notifications    | BOOLEAN   | YES  | TRUE    |                          | If enabled, the user receives email notifications                     |
| two_factor_enabled     | BOOLEAN   | YES  | FALSE   |                          | Whether two-factor authentication is enabled for added login security |
| last_password_change   | TIMESTAMP | YES  | NULL    |                          | Timestamp of the user's most recent password update                   |

---

### Relationships

| Related Table | Relationship Type | Foreign Key | Description                               |
| ------------- | ----------------- | ----------- | ----------------------------------------- |
| `users`       | One-to-One        | user\_id    | Each user has exactly one settings record |

---

### Example Record

```json
{
  "user_id": "c7b58c1d-30fa-43e3-80e3-fc4140f6832d",
  "is_private": true,
  "email_notifications": true,
  "two_factor_enabled": false,
  "last_password_change": "2025-06-20T09:30:00Z"
}
```

---

### Usage Scenarios

* **User privacy control**: Hide/show user profile or data from public or other users.
* **Notification settings**: Enable or disable system-generated emails, newsletters, and announcements.
* **Account security**: Enforce two-factor authentication if enabled.
* **Security audits**: Use `last_password_change` to monitor outdated passwords or trigger reminders.
* **Onboarding/Setup flow**: Automatically create a `user_settings` row upon registration.

---

### Query Examples

#### Get a user’s settings

```sql
SELECT * 
FROM user_settings
WHERE user_id = 'c7b58c1d-30fa-43e3-80e3-fc4140f6832d';
```

---

#### Get all users with two-factor enabled

```sql
SELECT user_id
FROM user_settings
WHERE two_factor_enabled = TRUE;
```

---

#### Get users who disabled email notifications

```sql
SELECT user_id
FROM user_settings
WHERE email_notifications = FALSE;
```

---

#### Find users who haven't changed password in last 6 months

```sql
SELECT user_id
FROM user_settings
WHERE last_password_change IS NULL
   OR last_password_change < CURRENT_DATE - INTERVAL '6 months';
```

---

### Insert Example

```sql
INSERT INTO user_settings (
  user_id,
  is_private,
  email_notifications,
  two_factor_enabled,
  last_password_change
) VALUES (
  'c7b58c1d-30fa-43e3-80e3-fc4140f6832d',
  TRUE,
  TRUE,
  FALSE,
  '2025-06-20T09:30:00Z'
);
```

---

### Best Practices & Notes

* **Create a row on user registration** to ensure every user has settings from day one.
* **Enforce security policies** based on `two_factor_enabled` and `last_password_change`.
* **Don't use NULL for booleans** unless necessary; always provide defaults (`FALSE`, `TRUE`) for clarity.
* **Join frequently with `users`** for access control or settings-based filtering.

---