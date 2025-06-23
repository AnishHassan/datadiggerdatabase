# Table: `subscriptions`

### **Description**

Tracks each user's active subscription on the platform. A user can have **only one active subscription at a time**, and they can upgrade, downgrade, or cancel their plan as needed. Payment details, renewal cycles, and subscription status are all managed here.

---

## **Schema**

| Column Name    | Data Type     | Null | Constraints                  | Description                                                                  |
| -------------- | ------------- | ---- | ---------------------------- | ---------------------------------------------------------------------------- |
| `id`           | INT           | NO   | Primary Key                  | Unique identifier for each subscription                                      |
| `user_id`      | UUID          | NO   | Unique, Foreign Key          | References `users.id`; one-to-one relationship per user                      |
| `plan_type`    | VARCHAR(50)   | YES  | -                            | Subscription tier (e.g., `basic`, `pro`, `enterprise`)                       |
| `payment_type` | VARCHAR(50)   | YES  | -                            | Payment provider/method (e.g., `stripe`, `paypal`)                           |
| `status`       | VARCHAR(50)   | YES  | -                            | Current subscription state (`active`, `trial`, `canceled`, etc.)             |
| `cost`         | NUMERIC(10,2) | YES  | -                            | Cost of the subscription in AUD (can be extended for multi-currency support) |
| `renewal_date` | DATE          | YES  | -                            | Date when the subscription is next due for renewal                           |
| `created_at`   | TIMESTAMP     | NO   | Default: `CURRENT_TIMESTAMP` | Timestamp of subscription creation                                           |
| `updated_at`   | TIMESTAMP     | NO   | Default: `CURRENT_TIMESTAMP` | Timestamp of last subscription update                                        |

---

## **Relationships**

| Related Table | Relationship Type | Foreign Key | Description                                     |
| ------------- | ----------------- | ----------- | ----------------------------------------------- |
| `users`       | One-to-One        | `user_id`   | Each user can have only one subscription record |

---

## **Example Record**

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
```

---

## **Usage Scenarios**

* Track and manage user subscription plans and statuses (active, trial, canceled).
* Determine renewal schedules and trigger payment workflows accordingly.
* Analyze revenue by subscription tier and status.
* Monitor churn, upgrades, and downgrades via `updated_at`.

---

## **Query Examples**

### Get all active subscriptions:

```sql
SELECT user_id, plan_type, renewal_date
FROM subscriptions
WHERE status = 'active';
```

### Find subscriptions renewing in the next 7 days:

```sql
SELECT user_id, plan_type, renewal_date
FROM subscriptions
WHERE status = 'active'
  AND renewal_date <= CURRENT_DATE + INTERVAL '7 days';
```

### Monthly revenue projection:

```sql
SELECT SUM(cost) AS monthly_revenue
FROM subscriptions
WHERE status = 'active'
  AND EXTRACT(MONTH FROM renewal_date) = 7
  AND EXTRACT(YEAR FROM renewal_date) = 2025;
```

### Identify canceled subscriptions in the last 30 days:

```sql
SELECT user_id, updated_at
FROM subscriptions
WHERE status = 'canceled'
  AND updated_at >= CURRENT_DATE - INTERVAL '30 days';
```

---

## **Insert Example**

```sql
INSERT INTO subscriptions (
  user_id, plan_type, payment_type, status, cost, renewal_date
) VALUES (
  'b1e4d4f6-b0c9-4c13-a6e9-925a24971fd1',
  'pro',
  'stripe',
  'active',
  29.99,
  '2025-07-21'
);
```

---
