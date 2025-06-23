# Table: `user_notification_channels`

### **Purpose**

Tracks **user-specific delivery preferences** for different types of notification channels (e.g., email, SMS, in-app, push). This enables fine-grained control over **how notifications are delivered** per user, and works in conjunction with broader `user_notification_settings`.

---

## Schema

| Column Name | Data Type | Null | Default | Constraints | Description                                                |
| ----------- | --------- | ---- | ------- | ----------- | ---------------------------------------------------------- |
| `id`        | UUID      | NO   | —       | Primary Key | Unique identifier for each notification channel preference |
| `user_id`   | UUID      | YES  | —       | Foreign Key | References the user who owns the setting                   |
| `channel`   | TEXT      | YES  | —       | —           | Notification delivery channel (e.g. `'email'`, `'sms'`)    |
| `enabled`   | BOOLEAN   | YES  | `true`  | —           | Indicates if this channel is active for the user           |

---

## Notes

* Common `channel` values: `'email'`, `'sms'`, `'in-app'`, `'push'`, `'webhook'`
* It's strongly recommended to **enforce uniqueness** on `(user_id, channel)` to prevent duplicate entries.
* Can be extended in the future with fields like `delivery_window`, `last_used_at`, or `priority`.

---

## Relationships

| Related Table                | Foreign Key     | Description                                           |
| ---------------------------- | --------------- | ----------------------------------------------------- |
| `users` (or `user_profiles`) | `user_id`       | Each channel setting is tied to one user              |
| `user_notification_settings` | (complementary) | Stores fine-grained preferences per notification type |

---

## Suggested Indexes

| Index Name                            | Columns                | Type  | Purpose                                            |
| ------------------------------------- | ---------------------- | ----- | -------------------------------------------------- |
| `user_notification_channels_pkey`     | `id`                   | BTREE | Ensures unique ID                                  |
| `user_notification_channels_uid_chan` | (`user_id`, `channel`) | BTREE | Prevents duplicates and improves query performance |

---

## Example Record

```json
{
  "id": "4e5a7a1c-2c36-4cf5-b5fc-e90253c73c56",
  "user_id": "e1f81d3d-b22f-470a-8d3c-4f03fbc347ab",
  "channel": "email",
  "enabled": true
}
```

---

## Query Examples

**Get all enabled notification channels for a user:**

```sql
SELECT channel
FROM user_notification_channels
WHERE user_id = 'e1f81d3d-b22f-470a-8d3c-4f03fbc347ab'
  AND enabled = true;
```

**Disable a specific channel (e.g. SMS) for a user:**

```sql
UPDATE user_notification_channels
SET enabled = false
WHERE user_id = 'e1f81d3d-b22f-470a-8d3c-4f03fbc347ab'
  AND channel = 'sms';
```

**Insert new notification channel preference:**

```sql
INSERT INTO user_notification_channels (id, user_id, channel, enabled)
VALUES (
  gen_random_uuid(),
  'e1f81d3d-b22f-470a-8d3c-4f03fbc347ab',
  'push',
  true
);
```

---

## Potential Enhancements

| Field           | Type      | Purpose                                                                |
| --------------- | --------- | ---------------------------------------------------------------------- |
| `last_used_at`  | TIMESTAMP | Track last time this channel was used for this user                    |
| `custom_config` | JSONB     | Store per-channel options (e.g., sender domain for email)              |
| `priority`      | SMALLINT  | Control delivery fallback (e.g., try push first, then SMS)             |
| `verified`      | BOOLEAN   | Mark channel as verified (e.g., phone or email confirmation completed) |

---