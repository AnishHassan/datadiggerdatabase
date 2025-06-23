# Table: `payment_logs`

---

## Description

Stores logs of all **payment transactions** initiated by users.
Tracks the **amount**, **plan type**, **payment method**, and **status** for each transaction, regardless of success or failure—ensuring complete financial traceability and auditability.

---

## Schema

| Column Name      | Data Type     | Null | Default             | Constraints | Description                                                      |
| ---------------- | ------------- | ---- | ------------------- | ----------- | ---------------------------------------------------------------- |
| `id`             | INT           | NO   | –                   | Primary Key | Unique identifier for each payment record                        |
| `user_id`        | UUID          | YES  | `NULL`              | Foreign Key | References the user making the payment (`users.id`)              |
| `amount`         | NUMERIC(10,2) | YES  | `NULL`              |             | Payment amount (supports up to 99999999.99)                      |
| `plan_type`      | VARCHAR(50)   | YES  | `NULL`              |             | Name of the plan purchased (e.g., `Free Trial`, `Pro Monthly`)   |
| `payment_method` | VARCHAR(50)   | YES  | `NULL`              |             | Payment method used (e.g., `Credit Card`, `Stripe`, `PayPal`)    |
| `status`         | VARCHAR(50)   | YES  | `NULL`              |             | Status of the transaction (`success`, `failed`, `pending`, etc.) |
| `created_at`     | TIMESTAMP     | NO   | `CURRENT_TIMESTAMP` |             | Timestamp when the payment was recorded                          |

---

## Relationships

| Related Table | Relationship Type | Foreign Key | Description                           |
| ------------- | ----------------- | ----------- | ------------------------------------- |
| `users`       | Many-to-One       | `user_id`   | A user can have multiple payment logs |

---

## Business Rules

* All **payment attempts** must be logged—regardless of outcome—for **auditing** and **fraud analysis**.
* Consider **normalizing** `plan_type` and `payment_method` into lookup/reference tables if values become repetitive or managed elsewhere.
* Use `status` to track payment pipeline stages (`pending → success/failed`).
* Nullable `user_id` allows anonymous or failed guest transactions to be recorded, if applicable.

---

## Indexes

| Index Name                 | Column    | Type  | Description                                            |
| -------------------------- | --------- | ----- | ------------------------------------------------------ |
| `idx_payment_logs_user_id` | `user_id` | BTREE | Optimizes user-specific payment queries                |
| `idx_payment_logs_status`  | `status`  | BTREE | Improves filtering and reporting by transaction status |

---

## Example Record

```json
{
  "id": 2048,
  "user_id": "b39dfab1-3149-4a75-9b6a-843e637f93c2",
  "amount": 49.99,
  "plan_type": "Pro Monthly",
  "payment_method": "Stripe",
  "status": "success",
  "created_at": "2025-06-21T11:32:00Z"
}
```

---

## Query Examples

### Recent Successful Payments

```sql
SELECT user_id, amount, plan_type, payment_method, created_at
FROM payment_logs
WHERE status = 'success'
ORDER BY created_at DESC
LIMIT 10;
```

### Total Revenue by Plan Type

```sql
SELECT plan_type, SUM(amount) AS total_revenue
FROM payment_logs
WHERE status = 'success'
GROUP BY plan_type
ORDER BY total_revenue DESC;
```
