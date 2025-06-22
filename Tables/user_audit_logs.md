# Table: `user_audit_logs`

# Description

Logs important user-related actions for auditing and tracking purposes. Useful for security, compliance, and activity monitoring.

---

# Schema

| Column Name | Data Type    | Null | Default             | Constraints | Description                                                      |
| ----------- | ------------ | ---- | ------------------- | ----------- | ---------------------------------------------------------------- |
| id          | UUID         | NO   | `gen_random_uuid()` | Primary Key | Unique identifier for the audit log entry                        |
| user_id     | UUID         | YES  | NULL                | Foreign Key | References the user who performed the action                     |
| action      | VARCHAR(100) | YES  | NULL                |             | Describes the type of action performed (e.g., `login`, `update`) |
| metadata    | JSONB        | YES  | NULL                |             | Additional contextual data (e.g., IP address, changed fields)    |
| created_at  | TIMESTAMP    | NO   | CURRENT\_TIMESTAMP  |             | Timestamp of when the action was recorded                        |

---

# Relationships

| Related Table | Relationship Type | Foreign Key | Description                            |
| ------------- | ----------------- | ----------- | -------------------------------------- |
| users         | Many-to-One       | user_id     | A user can have many audit log entries |

---

# Business Rules

* Each action logged should contain sufficient metadata to understand the event context.
* Use for tracking user-critical operations such as login attempts, password changes, etc.
* Ensure sensitive metadata (e.g., passwords) are never stored in raw form.

---

# Indexes

> *(Consider adding these if query performance becomes a concern)*

| Index Name                    | Column   | Type  | Description                                |
| ----------------------------- | -------- | ----- | ------------------------------------------ |
| `idx_user_audit_logs_user_id` | user_id  | BTREE | Speeds up queries filtering by user        |
| `idx_user_audit_logs_action`  | action   | BTREE | Speeds up queries filtering by action type |

---

# Example Row

```json
{
  "id": "7f1e3d98-3240-4e58-bb57-93e219daaa10",
  "user_id": "d34f6e99-d0b6-47a7-aed9-2fbd69319852",
  "action": "password_reset",
  "metadata": {
    "ip": "192.168.0.101",
    "method": "email_link"
  },
  "created_at": "2025-06-21T14:12:00Z"
}
```