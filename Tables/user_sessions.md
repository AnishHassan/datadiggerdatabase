# Table: `user_sessions`

---

### Description

Tracks user login sessions using secure refresh tokens and metadata such as IP address and user agent. Supports robust, token-based authentication, device tracking, and session expiry control for a secure multi-device experience.

---

### Schema

| Column Name    | Data Type | Null | Default             | Constraints | Description                                             |
| -------------- | --------- | ---- | ------------------- | ----------- | ------------------------------------------------------- |
| id             | UUID      | NO   | `gen_random_uuid()` | Primary Key | Unique identifier for the session                       |
| user_id        | UUID      | YES  | NULL                | Foreign Key | References the user who owns the session                |
| refresh_token  | TEXT      | NO   | NULL                | Unique      | Secure token used to refresh access credentials         |
| user_agent     | TEXT      | YES  | NULL                |             | Browser/device string associated with this session      |
| ip_address     | TEXT      | YES  | NULL                |             | IP address from which the session originated            |
| created_at     | TIMESTAMP | NO   | `CURRENT_TIMESTAMP` |             | Timestamp when the session was created                  |
| expires_at     | TIMESTAMP | YES  | NULL                |             | Optional expiration timestamp; null means no expiration |

---

### Relationships

| Related Table | Relationship Type | Foreign Key | Description                              |
| ------------- | ----------------- | ----------- | ---------------------------------------- |
| `users`       | Many-to-One       | user\_id    | A user can have multiple active sessions |

---

### Example Record

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

---

### Usage Scenarios

* **Multi-device login**: Track sessions across different devices for a single user.
* **Secure refresh flow**: Use `refresh_token` to issue new access tokens without reauthentication.
* **Session expiry**: Automatically invalidate old sessions via `expires_at`.
* **Suspicious login detection**: Analyze `user_agent` and `ip_address` changes.
* **Session management UI**: Let users review and revoke their active sessions.

---

### Query Examples

#### Get all active sessions for a user

```sql
SELECT * 
FROM user_sessions 
WHERE user_id = 'd728fc6b-c00d-44f0-973a-2bc72a34748a'
  AND (expires_at IS NULL OR expires_at > CURRENT_TIMESTAMP);
```

---

#### Find sessions from a suspicious IP

```sql
SELECT *
FROM user_sessions
WHERE ip_address = '203.0.113.42';
```

---

#### Cleanup expired sessions

```sql
DELETE FROM user_sessions
WHERE expires_at IS NOT NULL AND expires_at <= CURRENT_TIMESTAMP;
```

---

#### Revoke all other sessions for user (logout everywhere except current session)

```sql
DELETE FROM user_sessions
WHERE user_id = 'd728fc6b-c00d-44f0-973a-2bc72a34748a'
  AND id != 'abf4a3fa-29cb-4dfb-8412-7d7b0df46f8a';
```

---

### Insert Example

```sql
INSERT INTO user_sessions (
  id,
  user_id,
  refresh_token,
  user_agent,
  ip_address,
  created_at,
  expires_at
)
VALUES (
  gen_random_uuid(),
  'd728fc6b-c00d-44f0-973a-2bc72a34748a',
  'v2.lXcI6NzA9xI1YiZHUt1Z9cBvqZb4sZ',
  'Mozilla/5.0 (Windows NT 10.0; Win64; x64)',
  '192.168.0.103',
  CURRENT_TIMESTAMP,
  CURRENT_TIMESTAMP + INTERVAL '30 days'
);
```

---

### Optimizations & Notes

* **Security Tip**: Store `refresh_token` as a hash (e.g., SHA256) instead of plaintext.
* **Index on `refresh_token`** recommended for quick validation.
* **Regular cleanup** of expired tokens improves performance and security.
* **Add a `revoked_at` column** for soft deletion if audit trail is needed.

---
