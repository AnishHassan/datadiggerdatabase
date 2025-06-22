# Table: `notifications`

## Description

Stores notification messages intended for system-wide delivery or targeted users. These can include alerts, system messages, announcements, or updates sent through various channels (e.g., email, in-app).

---

## Schema

| Column Name   | Data Type | Null | Default | Constraints | Description                                                      |
| ------------- | --------- | ---- | ------- | ----------- | ---------------------------------------------------------------- |
| id            | UUID      | NO   | -       | Primary Key | Unique identifier for the notification                           |
| type          | TEXT      | NO   | -       |             | Type of notification (e.g., `info`, `warning`, `alert`, `promo`) |
| title         | TEXT      | NO   | -       |             | Title or headline of the notification                            |
| message       | TEXT      | YES  | -       |             | Body message of the notification                                 |
| metadata      | JSONB     | YES  | -       |             | Additional metadata (e.g., URLs, button actions, tags)           |
| send_to_all   | BOOL      | YES  | `false` |             | Whether the notification is broadcast to all users               |
| created_by    | UUID      | YES  | -       | Foreign Key | References the user/admin who created the notification           |
| created_at    | TIMESTAMP | YES  | `now()` |             | Timestamp when the notification was created                      |

---

## Relationships

| Related Table      | Relationship Type | Foreign Key  | Description                                 |
| ------------------ | ----------------- | ------------ | ------------------------------------------- |
| (optional) `users` | Many-to-One       | `created_by` | Who generated or initiated the notification |

---

## Business Rules

* `send_to_all = true` overrides any targeting and pushes the notification to all active users.
* `metadata` can be used for context-sensitive actions (e.g., `{ "link": "/dashboard", "priority": "high" }`).
* All system-triggered alerts should store a type (e.g., `system_error`, `policy_update`).

---

## Indexes

| Index Name           | Column(s)   | Type  | Description                                    |
| -------------------- | ----------- | ----- | ---------------------------------------------- |
| `notifications_pkey` | id          | BTREE | Primary key for fast lookup                    |
| (optional)           | created_by  | BTREE | For tracking notifications created by admins   |
| (optional)           | created_at  | BTREE | Useful for sorting and filtering recent alerts |

---

## Example Row

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