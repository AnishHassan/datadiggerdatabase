# Table: `user_audit_logs`

### **Description**

Captures sensitive, security-relevant, or compliance-mandated user activity logs. Focuses on **traceability**, **forensic analysis**, and **access pattern auditing**. Especially useful for:

* Investigating suspicious behavior
* Ensuring regulatory compliance (e.g., GDPR, HIPAA)
* Supporting incident response

---

## **Schema**

| Column Name  | Data Type    | Null | Default             | Constraints               | Description                                                          |
| ------------ | ------------ | ---- | ------------------- | ------------------------- | -------------------------------------------------------------------- |
| `id`         | UUID         | NO   | `gen_random_uuid()` | Primary Key               | Unique identifier for the audit log entry                            |
| `user_id`    | UUID         | YES  | NULL                | Foreign Key → `users(id)` | Identifies the user who performed the action                         |
| `action`     | VARCHAR(100) | YES  | NULL                |                           | Describes what was done (e.g., `login`, `update`, `delete_account`)  |
| `metadata`   | JSONB        | YES  | NULL                |                           | Key-value store of additional context (e.g., IP address, user agent) |
| `created_at` | TIMESTAMP    | NO   | `CURRENT_TIMESTAMP` |                           | UTC timestamp of when the action was logged                          |

---

## **Relationships**

| Related Table | Relationship Type | Foreign Key | Description                                      |
| ------------- | ----------------- | ----------- | ------------------------------------------------ |
| `users`       | Many-to-One       | `user_id`   | Links the log to a specific user (if applicable) |

---

## **Business Rules**

* Logs should include **enough metadata** to reconstruct what happened and from where.
* Never store **raw credentials**, **PII**, or **sensitive tokens** in the `metadata`.
* Actions should represent **meaningful security or compliance events**, such as:

  * Authentication events (`login`, `logout`, `2fa_enabled`)
  * Account lifecycle actions (`account_created`, `deactivated`, `password_changed`)
  * Permissions or role changes (`role_updated`)
  * Data exports or deletions

---

## **Example Actions**

| Action Name             | Description                                     |
| ----------------------- | ----------------------------------------------- |
| `login`                 | User login attempt (successful or failed)       |
| `logout`                | User logged out                                 |
| `password_reset`        | Password reset flow initiated or completed      |
| `profile_updated`       | User updated their profile settings             |
| `2fa_enabled`           | Two-factor authentication was turned on         |
| `account_deleted`       | Account was deleted by the user or admin        |
| `data_export_requested` | User requested an export of their personal data |

---

## **Example Row**

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

---

## **Indexing Recommendations**

| Index Name                    | Column            | Description                               |
| ----------------------------- | ----------------- | ----------------------------------------- |
| `idx_user_audit_logs_user_id` | `user_id`         | Fast lookup of logs for a specific user   |
| `idx_user_audit_logs_action`  | `action`          | Support filtering/grouping by action type |
| `idx_user_audit_logs_time`    | `created_at DESC` | Optimize recent-event sorting             |

---

## **Query Examples**

**Audit all login attempts from a user:**

```sql
SELECT * FROM user_audit_logs
WHERE user_id = 'd34f6e99-d0b6-47a7-aed9-2fbd69319852'
  AND action = 'login'
ORDER BY created_at DESC;
```
**Find all metadata related to data exports in the past 7 days:**

```sql
SELECT metadata, created_at FROM user_audit_logs
WHERE action = 'data_export_requested'
  AND created_at > now() - interval '7 days';
```

---

## **Security Best Practices**

* Apply **role-based access control (RBAC)** to restrict read access to this table.
* Use **immutable logs** (append-only model) if required for compliance.
* Ensure proper **encryption at rest and in transit** for this data.
* **Monitor and alert** on suspicious patterns, e.g., repeated failed logins, high-volume deletions.

---