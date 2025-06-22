# Table: `subscriptions`

# Description
Tracks each user's active subscription on the platform. A user can have **only one active subscription at a time**, and they can upgrade, downgrade, or cancel their plan as needed. All payment and renewal details are logged here.

---

# Schema

| Column Name    | Data Type       | Null | Default             | Constraints        | Description                                                      |
|----------------|-----------------|------|----------------------|--------------------|------------------------------------------------------------------|
| id             | INT             | NO   | -                    | Primary Key        | Unique identifier for each subscription                          |
| user_id        | UUID            | NO   | -                    | Unique             | Foreign key to `users.id`, each user can have only one subscription |
| plan_type      | VARCHAR(50)     | YES  | NULL                 |                    | Type of plan (e.g. `basic`, `pro`, `enterprise`)                 |
| payment_type   | VARCHAR(50)     | YES  | NULL                 |                    | Payment method (e.g. `credit_card`, `paypal`, `stripe`)          |
| status         | VARCHAR(50)     | YES  | NULL                 |                    | Current subscription status (e.g. `active`, `canceled`, `trial`) |
| cost           | NUMERIC(10,2)   | YES  | NULL                 |                    | Subscription cost in AUD (for AU/NZ launch), can adapt globally  |
| renewal_date   | DATE            | YES  | NULL                 |                    | Date when the next payment is due                                |
| created_at     | TIMESTAMP       | NO   | CURRENT_TIMESTAMP    |                    | Timestamp when the subscription was created                      |
| updated_at     | TIMESTAMP       | NO   | CURRENT_TIMESTAMP    |                    | Timestamp when the subscription was last updated                 |

---

# Relationships

| Related Table | Relationship Type | Foreign Key | Description                              |
|---------------|-------------------|-------------|------------------------------------------|
| users         | One-to-One        | user_id     | Each user can have only one subscription |

---

# Business Rules

- Each user can have only **one active subscription**.
- Use `updated_at` to track plan changes like upgrades or downgrades.
- Status values should be managed via an ENUM or controlled list (`active`, `canceled`, etc.).
- Currency handling should be localized for future international rollout.

---

# Example Row

```json
{
  "id": 103,
  "user_id": "b1e4d4f6-b0c9-4c13-a6e9-925a24971fd1",
  "plan_type": "pro",
  "payment_type": "stripe",
  "status": "active",
  "cost": 29.99,
  "renewal_date": "2025-07-21",
  "created_at": "2025-06-21T08:35:00Z",
  "updated_at": "2025-06-21T08:35:00Z"
}
