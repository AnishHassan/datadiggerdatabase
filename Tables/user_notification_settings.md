# Table: `user_notification_settings`

### **Purpose**

Manages **per-user preferences for notification types** (e.g., messages, system alerts, digests), enabling users to control *what* kinds of notifications they want to receive.

---

## Schema

| Column Name         | Data Type | Null | Default | Constraints | Description                                               |
| ------------------- | --------- | ---- | ------- | ----------- | --------------------------------------------------------- |
| `id`                | UUID      | NO   | —       | Primary Key | Unique identifier for the setting                         |
| `user_id`           | UUID      | YES  | —       | Foreign Key | User to whom this notification setting applies            |
| `notification_type` | TEXT      | YES  | —       | —           | Type/category of notification (e.g. `'alert'`, `'promo'`) |
| `enabled`           | BOOLEAN   | YES  | `true`  | —           | Whether this type of notification is enabled for the user |

---

## Notes

* Recommended to **enforce uniqueness** on `(user_id, notification_type)` to prevent duplicate records.
* `notification_type` should ideally be **enumerated** via:

  * **application constants**, or
  * a foreign key to a lookup table like `notification_types(id, name, description)`
* Use in conjunction with `user_notification_channels` to determine *how* enabled notifications are delivered.

---

## Relationships

| Related Table                   | Foreign Key         | Description                                  |
| ------------------------------- | ------------------- | -------------------------------------------- |
| `users`                         | `user_id`           | Identifies which user the setting belongs to |
| `notification_types` (optional) | `notification_type` | If normalized, provides metadata per type    |

---

## Suggested Indexes

| Index Name                            | Columns                          | Type  | Purpose                                 |
| ------------------------------------- | -------------------------------- | ----- | --------------------------------------- |
| `user_notification_settings_pkey`     | `id`                             | BTREE | Primary key                             |
| `user_notification_settings_uid_type` | (`user_id`, `notification_type`) | BTREE | Prevent duplicates and speed up lookups |

---

## Example Record

```json
{
  "id": "0a135dc4-bc77-4bc0-a54e-4e1c5e2d41b0",
  "user_id": "c981b8e3-e34f-44c5-a6a7-1cabe3a7be12",
  "notification_type": "daily_summary",
  "enabled": true
}
```

---

## Query Examples

**Enable all notifications for a user:**

```sql
UPDATE user_notification_settings
SET enabled = true
WHERE user_id = 'c981b8e3-e34f-44c5-a6a7-1cabe3a7be12';
```

**Get all disabled notification types for a user:**

```sql
SELECT notification_type
FROM user_notification_settings
WHERE user_id = 'c981b8e3-e34f-44c5-a6a7-1cabe3a7be12'
  AND enabled = false;
```

**Add a new setting (e.g., weekly digest):**

```sql
INSERT INTO user_notification_settings (id, user_id, notification_type, enabled)
VALUES (
  gen_random_uuid(),
  'c981b8e3-e34f-44c5-a6a7-1cabe3a7be12',
  'weekly_digest',
  true
);
```

---

## Potential Enhancements

| Field        | Type      | Purpose                                                               |
| ------------ | --------- | --------------------------------------------------------------------- |
| `frequency`  | TEXT      | Allow user to set delivery rate (e.g. `'immediate'`, `'daily'`, etc.) |
| `created_at` | TIMESTAMP | Audit trail: when the preference was set                              |
| `updated_at` | TIMESTAMP | Track when the setting was last modified                              |
| `source`     | TEXT      | Indicates how the preference was set (e.g., `'web'`, `'admin'`)       |

---