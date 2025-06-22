# Table: `user_notifications`

## Description

Tracks the delivery and interaction status of notifications for individual users. Used to determine whether a user has received, read, or dismissed a notification.

---

## Schema

| Column Name           | Data Type | Null | Default | Constraints | Description                                                 |
| --------------------- | --------- | ---- | ------- | ----------- | ----------------------------------------------------------- |
| id                    | UUID      | NO   | -       | Primary Key | Unique identifier for the user-notification record          |
| user_id               | UUID      | YES  | -       | Foreign Key | ID of the user who receives the notification                |
| notification_id       | UUID      | YES  | -       | Foreign Key | ID of the related notification                              |
| is_read               | BOOL      | YES  | `false` |             | Whether the user has marked the notification as read        |
| is_dismissed          | BOOL      | YES  | `false` |             | Whether the user dismissed (closed or hid) the notification |
| notification_enabled  | BOOL      | YES  | `true`  |             | Whether this notification is enabled for the user           |
| delivered_at          | TIMESTAMP | YES  | `now()` |             | Timestamp when the notification was delivered to the user   |

---

## Relationships

| Related Table   | Relationship Type | Foreign Key       | Description                            |
| --------------- | ----------------- | ----------------- | -------------------------------------- |
| `users`         | Many-to-One       | `user_id`         | The user who receives the notification |
| `notifications` | Many-to-One       | `notification_id` | The notification being tracked         |

---

## Business Rules

* A notification will only appear to the user if `notification_enabled = true`.
* `is_read` and `is_dismissed` are independent flags — a user may dismiss without reading or vice versa.
* `delivered_at` can be used to measure notification delivery performance or delay.

---

## Indexes (Suggested)

| Index Name                       | Column(s)                  | Type  | Description                                   |
| -------------------------------- | -------------------------- | ----- | --------------------------------------------- |
| `user_notifications_pkey`        | id                         | BTREE | Primary key                                   |
| `user_notifications_uid_nid_idx` | user_id, notification_id   | BTREE | Fast lookup of user-notification associations |
| `user_notifications_read_idx`    | user_id, is_read           | BTREE | Filter unread notifications quickly           |

---

## Example Row

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