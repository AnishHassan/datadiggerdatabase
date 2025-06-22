# Table: `user_notification_channels`

### Description:

Stores user-specific preferences for different notification delivery channels (e.g., email, SMS, in-app).

---

## Schema

| Column Name | Data Type | Null | Default | Constraints | Description                                                  |
| ----------- | --------- | ---- | ------- | ----------- | ------------------------------------------------------------ |
| id          | UUID      | NO   | -       | Primary Key | Unique identifier for the notification channel record        |
| user_id     | UUID      | YES  | -       | -           | References the user who owns this channel setting            |
| channel     | TEXT      | YES  | -       | -           | Type of notification channel (e.g. `email`, `sms`, `in-app`) |
| enabled     | BOOLEAN   | YES  | `true`  | -           | Indicates whether the channel is active for this user        |

---

## Notes

* Suggested `channel` values might include: `"email"`, `"sms"`, `"in-app"`, `"push"`, etc.
* You may want to add a unique constraint on `(user_id, channel)` to avoid duplicate entries.
* This table works in conjunction with `user_notification_settings` to fully manage notification preferences.

---

## Suggested Indexes

| Index Name                            | Columns             | Type  | Purpose                                 |
| ------------------------------------- | ------------------- | ----- | --------------------------------------- |
| `user_notification_channels_pkey`     | id                  | BTREE | Primary key                             |
| `user_notification_channels_uid_chan` | (user_id, channel)  | BTREE | Prevent duplicates and optimize lookups |

---

## Example Row

```json
{
  "id": "4e5a7a1c-2c36-4cf5-b5fc-e90253c73c56",
  "user_id": "e1f81d3d-b22f-470a-8d3c-4f03fbc347ab",
  "channel": "email",
  "enabled": true
}
```

---

## Query Example

**Get all enabled notification channels for a user:**

```sql
SELECT channel
FROM user_notification_channels
WHERE user_id = 'e1f81d3d-b22f-470a-8d3c-4f03fbc347ab'
  AND enabled = true;
```

**Disable SMS notifications for a specific user:**

```sql
UPDATE user_notification_channels
SET enabled = false
WHERE user_id = 'e1f81d3d-b22f-470a-8d3c-4f03fbc347ab'
  AND channel = 'sms';
```

---

## Insert Example

```sql
INSERT INTO user_notification_channels (id, user_id, channel, enabled)
VALUES (
  gen_random_uuid(),
  'e1f81d3d-b22f-470a-8d3c-4f03fbc347ab',
  'in-app',
  true
);
```