# Table: `user_notifications`

### **Purpose**

Tracks **notification delivery and user interaction** (read/dismissed status) for each user. This enables personalized notification UX, analytics, and delivery tracking.

---

## Schema

| Column Name            | Data Type | Null | Default | Constraints | Description                                                   |
| ---------------------- | --------- | ---- | ------- | ----------- | ------------------------------------------------------------- |
| `id`                   | UUID      | NO   | —       | Primary Key | Unique record per user-notification pairing                   |
| `user_id`              | UUID      | YES  | —       | Foreign Key | ID of the user receiving the notification                     |
| `notification_id`      | UUID      | YES  | —       | Foreign Key | ID of the notification (from `notifications` table)           |
| `is_read`              | BOOL      | YES  | `false` | —           | Whether the user has read the notification                    |
| `is_dismissed`         | BOOL      | YES  | `false` | —           | Whether the user dismissed/closed the notification            |
| `notification_enabled` | BOOL      | YES  | `true`  | —           | Whether this notification was enabled at the time of delivery |
| `delivered_at`         | TIMESTAMP | YES  | `now()` | —           | When the notification was delivered to the user               |

---

## Relationships

| Related Table   | Foreign Key       | Description                       |
| --------------- | ----------------- | --------------------------------- |
| `users`         | `user_id`         | Recipient of the notification     |
| `notifications` | `notification_id` | Notification content and metadata |

---

## Business Rules

* Only display notifications to users if `notification_enabled = true`.
* `is_read` and `is_dismissed` are **not mutually exclusive** (e.g., a user can read but not dismiss, or vice versa).
* `delivered_at` can support delivery latency metrics and notification engagement analysis.

---

## Suggested Indexes

| Index Name                         | Columns                        | Type  | Use Case                               |
| ---------------------------------- | ------------------------------ | ----- | -------------------------------------- |
| `user_notifications_pkey`          | `id`                           | BTREE | Enforces uniqueness                    |
| `user_notifications_uid_nid_idx`   | (`user_id`, `notification_id`) | BTREE | For deduplication and joins            |
| `user_notifications_read_idx`      | (`user_id`, `is_read`)         | BTREE | Fetch unread notifications efficiently |
| `user_notifications_dismissed_idx` | (`user_id`, `is_dismissed`)    | BTREE | Query dismissed notifications quickly  |

---

## Example Record

```json
{
  "id": "75f834b1-ff99-4a4d-a132-4f9869a291df",
  "user_id": "d2e6c932-df99-44b1-83f9-2bcb402fab22",
  "notification_id": "4825c814-df13-4050-9086-0f3c4d4b8a12",
  "is_read": false,
  "is_dismissed": false,
  "notification_enabled": true,
  "delivered_at": "2025-06-21T12:00:00Z"
}
```

---

## Query Examples

**Get unread notifications for a user:**

```sql
SELECT *
FROM user_notifications
WHERE user_id = 'd2e6c932-df99-44b1-83f9-2bcb402fab22'
  AND is_read = false
  AND notification_enabled = true;
```

**Mark notification as read:**

```sql
UPDATE user_notifications
SET is_read = true
WHERE user_id = 'd2e6c932-df99-44b1-83f9-2bcb402fab22'
  AND notification_id = '4825c814-df13-4050-9086-0f3c4d4b8a12';
```

**Count dismissed notifications per user:**

```sql
SELECT COUNT(*)
FROM user_notifications
WHERE user_id = 'd2e6c932-df99-44b1-83f9-2bcb402fab22'
  AND is_dismissed = true;
```

---

## Optional Enhancements

| Field                | Type      | Description                                                         |
| -------------------- | --------- | ------------------------------------------------------------------- |
| `read_at`            | TIMESTAMP | Records exact time the notification was read (audit/tracking)       |
| `dismissed_at`       | TIMESTAMP | Time when the user dismissed the notification                       |
| `channel`            | TEXT      | Channel used for delivery (e.g. `'email'`, `'in-app'`, `'sms'`)     |
| `delivery_status`    | TEXT      | `'delivered'`, `'failed'`, `'pending'` — useful for async channels  |
| `interaction_source` | TEXT      | E.g. `'web'`, `'mobile'`, `'api'` — how the user interacted with it |

---