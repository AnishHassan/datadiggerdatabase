# Table: `notifications`

---

## Description

Stores **notifications** intended for either system-wide delivery or targeting specific users.
Supports diverse types such as alerts, updates, promotional messages, or announcements, and supports delivery across multiple channels (e.g., **email**, **in-app**, **push**).

---

## Schema

| Column Name   | Data Type | Null | Default | Constraints | Description                                                               |
| ------------- | --------- | ---- | ------- | ----------- | ------------------------------------------------------------------------- |
| `id`          | UUID      | NO   | –       | Primary Key | Unique identifier for the notification                                    |
| `type`        | TEXT      | NO   | –       |             | Category/type of notification (e.g., `info`, `warning`, `alert`, `promo`) |
| `title`       | TEXT      | NO   | –       |             | Headline or title of the notification                                     |
| `message`     | TEXT      | YES  | –       |             | Main body/message of the notification                                     |
| `metadata`    | JSONB     | YES  | –       |             | Extra data such as links, CTA buttons, tags (e.g., `{ "link": "/home" }`) |
| `send_to_all` | BOOL      | YES  | `false` |             | If `true`, the notification is sent to all users                          |
| `created_by`  | UUID      | YES  | –       | Foreign Key | References the user/admin who initiated the notification (`users.id`)     |
| `created_at`  | TIMESTAMP | YES  | `now()` |             | Timestamp of when the notification was created                            |

---

## Relationships

| Related Table | Relationship Type | Foreign Key  | Description                                             |
| ------------- | ----------------- | ------------ | ------------------------------------------------------- |
| `users`       | Many-to-One       | `created_by` | Links to the user or admin who created the notification |

---

## Business Rules

* `send_to_all = true` overrides any user-specific targeting and pushes to **all active users**.
* `metadata` allows rich notifications with custom actions (e.g., deep linking, priorities).
* All system-generated notifications must have a `type` (e.g., `system_error`, `policy_update`).
* `created_by` may be null for automated or system-level messages.

---

## Indexes

| Index Name           | Column(s)    | Type  | Description                                       |
| -------------------- | ------------ | ----- | ------------------------------------------------- |
| `notifications_pkey` | `id`         | BTREE | Primary key for fast lookups                      |
| (optional)           | `created_by` | BTREE | Enables audit/logs by creator                     |
| (optional)           | `created_at` | BTREE | Useful for sorting/filtering recent notifications |

---

## Example Record

```json
{
  "id": "4825c814-df13-4050-9086-0f3c4d4b8a12",
  "type": "info",
  "title": "New Features Released",
  "message": "Check out the latest enhancements in your dashboard.",
  "metadata": {
    "link": "/features",
    "priority": "medium"
  },
  "send_to_all": true,
  "created_by": "631ae3d0-a84d-482f-b0bb-7c92b9b5be14",
  "created_at": "2025-06-21T11:45:00Z"
}
```

---

## Query Examples

### Get Recent Notifications Created by an Admin

```sql
SELECT title, message, created_at
FROM notifications
WHERE created_by = '631ae3d0-a84d-482f-b0bb-7c92b9b5be14'
ORDER BY created_at DESC;
```

### Fetch All Broadcast Messages

```sql
SELECT *
FROM notifications
WHERE send_to_all = true;
```