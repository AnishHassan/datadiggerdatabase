# Table: `user_notification_settings`

### Description:

Stores per-user preferences for enabling or disabling different types of notifications.

---

## Schema

| Column Name        | Data Type | Null | Default | Constraints | Description                                             |
| ------------------ | --------- | ---- | ------- | ----------- | ------------------------------------------------------- |
| id                 | UUID      | NO   | -       | Primary Key | Unique identifier for the notification setting record   |
| user_id            | UUID      | YES  | -       | -           | References the user associated with these settings      |
| notification_type  | TEXT      | YES  | -       | -           | The type/category of notification (e.g. alert, message) |
| enabled            | BOOLEAN   | YES  | `true`  | -           | Whether this notification type is enabled for the user  |

---

## Notes

* You can add a unique constraint on `(user_id, notification_type)` to prevent duplicate settings for the same type.
* `notification_type` values should be standardized/enumerated in the application logic or via a lookup table.
* `enabled = true` implies the user wants to receive this notification type.

---

## Suggested Indexes

| Index Name                            | Columns                        | Type  | Purpose                                 |
| ------------------------------------- | ------------------------------ | ----- | --------------------------------------- |
| `user_notification_settings_pkey`     | id                             | BTREE | Primary key                             |
| `user_notification_settings_uid_type` | (user_id, notification_type)   | BTREE | Prevent duplicate entries; fast lookups |

---

## Example Row

```json
{
  "id": "0a135dc4-bc77-4bc0-a54e-4e1c5e2d41b0",
  "user_id": "c981b8e3-e34f-44c5-a6a7-1cabe3a7be12",
  "notification_type": "daily_summary",
  "enabled": true
}
```

---

## Query Example

**Enable all notifications for a user:**

```sql
UPDATE user_notification_settings
SET enabled = true
WHERE user_id = 'c981b8e3-e34f-44c5-a6a7-1cabe3a7be12';
```

**Get disabled notification types for a user:**

```sql
SELECT notification_type
FROM user_notification_settings
WHERE user_id = 'c981b8e3-e34f-44c5-a6a7-1cabe3a7be12'
  AND enabled = false;
```

---

## Insert Example

```sql
INSERT INTO user_notification_settings (id, user_id, notification_type, enabled)
VALUES (
  gen_random_uuid(),
  'c981b8e3-e34f-44c5-a6a7-1cabe3a7be12',
  'weekly_digest',
  true
);
```