# Table: `login_logs`

# Description

Tracks every login attempt by users, capturing metadata such as IP address, device info, and location. Flags potentially suspicious login attempts for security monitoring.

---

# Schema

| Column Name | Data Type | Null | Default            | Constraints | Description                                       |
| ----------- | --------- | ---- | ------------------ | ----------- | ------------------------------------------------- |
| id          | INT       | NO   | -                  | Primary Key | Unique identifier for the login log entry         |
| user_id     | UUID      | YES  | NULL               | Foreign Key | References the user attempting to log in          |
| ip_address  | INET      | YES  | NULL               |             | IP address from which the login was attempted     |
| user_agent  | TEXT      | YES  | NULL               |             | Device or browser information of the login source |
| location    | TEXT      | YES  | NULL               |             | Geolocation data derived from the IP address      |
| suspicious  | BOOL      | NO   | `false`            |             | Indicates if the login is flagged as suspicious   |
| created_at  | TIMESTAMP | NO   | CURRENT_TIMESTAMP  |             | Timestamp of when the login attempt occurred      |

---

# Relationships

| Related Table | Relationship Type | Foreign Key | Description                        |
| ------------- | ----------------- | ----------- | ---------------------------------- |
| users         | Many-to-One       | user_id     | A user can have many login entries |

---

# Business Rules

* All login attempts should be logged, regardless of success or failure (if applicable).
* Suspicious logins (e.g. new IPs, geolocation anomalies) should set `suspicious = true`.
* `ip_address` and `user_agent` help with detecting bot or abuse activity.

---

# Indexes

> *(Optional — for performance tuning in production)*

| Index Name                  | Column      | Type  | Description                                 |
| --------------------------- | ----------- | ----- | ------------------------------------------- |
| `idx_login_logs_user_id`    | user_id     | BTREE | Speeds up filtering login history by user   |
| `idx_login_logs_created_at` | created_at  | BTREE | Improves performance for time-based queries |

---

# Example Row

```json
{
  "id": 984,
  "user_id": "f0e8dc59-1b61-4e0c-9c5a-df1c65d28e3c",
  "ip_address": "192.168.1.44",
  "user_agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64)",
  "location": "Berlin, Germany",
  "suspicious": false,
  "created_at": "2025-06-21T09:45:00Z"
}
```