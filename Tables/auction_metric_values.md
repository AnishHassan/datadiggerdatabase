# Table: `auction_metric_values`

### **Description**

Stores quantitative values of specific auction-related metrics for a given event. It links auction events with standardized metric codes and reported values (e.g., bid-to-cover ratio, total amount).

---

## Schema

| Column Name   | Data Type     | Null | Constraints                               | Description                                                |
| ------------- | ------------- | ---- | ----------------------------------------- | ---------------------------------------------------------- |
| `id`          | INT4          | NO   | Primary Key, Auto-Increment               | Unique record identifier                                   |
| `event_id`    | INT4          | NO   | Foreign Key → `[Auction Events Table].id` | Identifier for the auction event                           |
| `metric_code` | VARCHAR(32)   | YES  | –                                         | Code representing the metric (e.g., `BTC_RATIO`, `AMOUNT`) |
| `value`       | NUMERIC(18,6) | YES  | –                                         | Numerical value for the metric                             |
| `updated_at`  | TIMESTAMP     | YES  | Default: `CURRENT_TIMESTAMP`              | Timestamp when the value was last updated                  |

---

## Indexes

| Index Name        | Columns                     | Type  | Description                                             |
| ----------------- | --------------------------- | ----- | ------------------------------------------------------- |
| `uq_event_metric` | (`event_id`, `metric_code`) | BTREE | Ensures each metric appears only once per auction event |

---

## Key Relationships

* `event_id` → (assumed FK) to a referenced **`auction_events`** or similar events table.

---

## Usage Scenarios

Linking performance metrics (e.g., bid-to-cover ratio, total bids) to auction events.

Feeding data to dashboards and analytics tools for trend analysis.

Enforcing that each metric appears only once per auction event through a unique index.

Comparing metric values across different auctions for reporting or auditing.

---

## Example Record

```json
{
  "id": 2310,
  "event_id": 875,
  "metric_code": "BTC_RATIO",
  "value": 2.154321,
  "updated_at": "2025-06-22 10:45:00"
}
```

---

## Query Examples

### Get all metrics for a specific event:

```sql
SELECT metric_code, value
FROM auction_metric_values
WHERE event_id = 875;
```

# Aggregate average values for a metric:

```sql
SELECT metric_code, AVG(value) AS avg_value
FROM auction_metric_values
GROUP BY metric_code;
```

# Find the latest updated metric value per event:
```sql
SELECT event_id, metric_code, value, updated_at
FROM auction_metric_values
WHERE updated_at = (
  SELECT MAX(updated_at)
  FROM auction_metric_values amv2
  WHERE amv2.event_id = auction_metric_values.event_id
    AND amv2.metric_code = auction_metric_values.metric_code
);
```

# Insert Example

```sql
INSERT INTO auction_metric_values (event_id, metric_code, value)
VALUES (875, 'BTC_RATIO', 2.154321);
```
