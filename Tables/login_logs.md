# Table: `login_logs`

---

## **Description**

Captures metadata for all user login attempts, enabling session tracking, anomaly detection, and compliance with security auditing standards. Each record includes IP, device fingerprinting, geolocation, and a `suspicious` flag to assist with threat modeling and automated alerts.

---

## **Schema**

| Column Name  | Data Type | Null | Default             | Constraints               | Description                                                           |
| ------------ | --------- | ---- | ------------------- | ------------------------- | --------------------------------------------------------------------- |
| `id`         | INT       | NO   | –                   | Primary Key               | Unique identifier for each login event                                |
| `user_id`    | UUID      | YES  | NULL                | Foreign Key → `users(id)` | UUID of the user who attempted the login (nullable for unknown users) |
| `ip_address` | INET      | YES  | NULL                |                           | IP address from which the login attempt originated                    |
| `user_agent` | TEXT      | YES  | NULL                |                           | Device and browser details (e.g., user-agent string)                  |
| `location`   | TEXT      | YES  | NULL                |                           | Human-readable geolocation inferred from `ip_address`                 |
| `suspicious` | BOOL      | NO   | `false`             |                           | Flag indicating anomaly or risk detected in the login event           |
| `created_at` | TIMESTAMP | NO   | `CURRENT_TIMESTAMP` |                           | Timestamp of when the login event was logged (UTC)                    |

---

## **Relationships**

| Related Table | Relationship Type | Column    | Description                           |
| ------------- | ----------------- | --------- | ------------------------------------- |
| `users`       | Many-to-One       | `user_id` | Links login event to registered users |

---

## **Business Rules**

* Every login attempt (successful or failed) **must be logged**.
* IP geolocation and browser fingerprinting may be used to:

  * Trigger the `suspicious` flag on unfamiliar patterns (e.g., new countries or devices).
  * Power alerts, 2FA, or login verification flows.
* Anonymous logins (e.g., failed attempts with invalid user credentials) may have `user_id` as `NULL`.

---

## **Indexes (Performance Considerations)**

| Index Name                  | Column       | Index Type | Use Case                                   |
| --------------------------- | ------------ | ---------- | ------------------------------------------ |
| `idx_login_logs_user_id`    | `user_id`    | BTREE      | Retrieve login history for a specific user |
| `idx_login_logs_created_at` | `created_at` | BTREE      | Analyze login attempts over time           |

---

## **Example Record**

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

---

## **Usage Scenarios**

* **Security Analytics**: Identify brute-force or credential stuffing attempts.
* **User Support**: Audit login patterns during account recovery or fraud disputes.
* **Risk Scoring**: Combine `user_agent`, `location`, and `ip_address` for anomaly scoring.
* **Compliance**: Meet standards for logging user authentication events (e.g., SOC 2, GDPR logging).

---

## **Query Examples**

### Retrieve login history for a specific user:

```sql
SELECT *
FROM login_logs
WHERE user_id = 'f0e8dc59-1b61-4e0c-9c5a-df1c65d28e3c'
ORDER BY created_at DESC
LIMIT 50;
```

---

### Count suspicious logins by IP in the last 24 hours:

```sql
SELECT ip_address, COUNT(*) AS attempts
FROM login_logs
WHERE suspicious = true
  AND created_at >= NOW() - INTERVAL '1 day'
GROUP BY ip_address
ORDER BY attempts DESC;
```

---

### Find logins from new locations for a user:

```sql
SELECT DISTINCT location
FROM login_logs
WHERE user_id = 'f0e8dc59-1b61-4e0c-9c5a-df1c65d28e3c'
ORDER BY created_at DESC;
```