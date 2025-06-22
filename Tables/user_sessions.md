# Table: `user_sessions`

# Description

Tracks user login sessions, storing refresh tokens and metadata such as IP address and user agent. Supports secure token-based authentication and session expiry.

---

# Schema

| Column Name    | Data Type | Null | Default             | Constraints | Description                                   |
| -------------- | --------- | ---- | ------------------- | ----------- | --------------------------------------------- |
| id             | UUID      | NO   | `gen_random_uuid()` | Primary Key | Unique identifier for the session             |
| user_id        | UUID      | YES  | NULL                | Foreign Key | References the user who owns the session      |
| refresh_token  | TEXT      | NO   | NULL                |             | Refresh token tied to the session             |
| user_agent     | TEXT      | YES  | NULL                |             | Device/browser info for the session           |
| ip_address     | TEXT      | YES  | NULL                |             | IP address used to create the session         |
| created_at     | TIMESTAMP | NO   | CURRENT\_TIMESTAMP  |             | Timestamp when the session was created        |
| expires_at     | TIMESTAMP | YES  | NULL                |             | Optional expiration timestamp for the session |

---

# Relationships

| Related Table | Relationship Type | Foreign Key | Description                              |
| ------------- | ----------------- | ----------- | ---------------------------------------- |
| users         | Many-to-One       | user_id     | A user can have multiple active sessions |

---

# Business Rules

* Refresh tokens should be stored securely and treated as sensitive credentials.
* You may implement limits on the number of active sessions per user.
* Expired or invalidated sessions should be cleared periodically for security.
* Consider logging IP/user agent mismatches to detect suspicious activity.

---

# Indexes

| Index Name              | Column   | Type  | Description                                 |
| ----------------------- | -------- | ----- | ------------------------------------------- |
| `idx_user_sessions_uid` | user_id  | BTREE | Optimizes queries fetching sessions by user |

---

# Example Row

```json
{
  "id": "abf4a3fa-29cb-4dfb-8412-7d7b0df46f8a",
  "user_id": "d728fc6b-c00d-44f0-973a-2bc72a34748a",
  "refresh_token": "v2.lXcI6NzA9xI1YiZHUt1Z9cBvqZb4sZ",
  "user_agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64)",
  "ip_address": "192.168.0.103",
  "created_at": "2025-06-21T09:35:00Z",
  "expires_at": "2025-07-21T09:35:00Z"
}
```