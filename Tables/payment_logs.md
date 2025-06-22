# Table: `payment_logs`

# Description

Logs all payment transactions made by users, including the amount, plan type, method of payment, and transaction status.

---

# Schema

| Column Name     | Data Type     | Null | Default            | Constraints | Description                                                |
| --------------- | ------------- | ---- | ------------------ | ----------- | ---------------------------------------------------------- |
| id              | INT           | NO   | -                  | Primary Key | Unique identifier for the payment record                   |
| user_id         | UUID          | YES  | NULL               | Foreign Key | References the user making the payment                     |
| amount          | NUMERIC(10,2) | YES  | NULL               |             | Payment amount in applicable currency                      |
| plan_type       | VARCHAR(50)   | YES  | NULL               |             | The subscription or service plan purchased                 |
| payment_method  | VARCHAR(50)   | YES  | NULL               |             | Method used for the payment (e.g., credit card, PayPal)    |
| status          | VARCHAR(50)   | YES  | NULL               |             | Status of the transaction (e.g., success, failed, pending) |
| created_at      | TIMESTAMP     | NO   | CURRENT\_TIMESTAMP |             | Timestamp of when the payment was recorded                 |

---

# Relationships

| Related Table | Relationship Type | Foreign Key | Description                           |
| ------------- | ----------------- | ----------- | ------------------------------------- |
| users         | Many-to-One       | user_id     | A user can have multiple payment logs |

---

# Business Rules

* Every payment attempt—successful or not—should be logged for auditing.
* Consider normalizing `plan_type` and `payment_method` if the values are reused often.
* Use `status` to track transaction progress (e.g., pending, success, failed).

---

# Indexes

| Index Name                 | Column   | Type  | Description                                   |
| -------------------------- | -------- | ----- | --------------------------------------------- |
| `idx_payment_logs_user_id` | user_id  | BTREE | Improves performance for user payment queries |
| `idx_payment_logs_status`  | status   | BTREE | Speeds up filtering by payment status         |

---

# Example Row

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