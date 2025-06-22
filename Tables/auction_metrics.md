# Table: `auction_metrics`

### Description:

Stores metadata about various metrics used to analyze or report on auction calendar events, including their units, display preferences, and ordering.

---

## Schema

| Column Name       | Data Type   | Null | Default             | Constraints | Description                                                     |
| ----------------- | ----------- | ---- | ------------------- | ----------- | --------------------------------------------------------------- |
| metric_code       | VARCHAR(32) | NO   | -                   | Primary Key | Unique code identifying the metric (e.g., `yield`, `volume`)    |
| description       | TEXT        | NO   | -                   | Not Null    | Human-readable explanation of the metric                        |
| unit              | VARCHAR(16) | YES  | -                   | -           | Unit of measurement (e.g., `%`, `USD`, `bps`)                   |
| preferred_format  | VARCHAR(32) | YES  | -                   | -           | Formatting suggestion for displaying the metric (e.g., `0.00%`) |
| display_order     | INT4        | YES  | `0`                 | -           | Relative order for display in UI or reports                     |
| created_at        | TIMESTAMPTZ | NO   | `CURRENT_TIMESTAMP` | -           | Timestamp when the metric record was created                    |
| updated_at        | TIMESTAMPTZ | NO   | `CURRENT_TIMESTAMP` | -           | Timestamp when the metric record was last updated               |

---

## Notes

* `metric_code` should be unique and used as a reference key when associating metrics with auction data points.
* `preferred_format` helps define how to present values (e.g., percentage, currency, or decimal).
* `display_order` can be used for sorting metrics in tables or charts.

---

## Suggested Indexes

| Index Name             | Columns      | Type  | Purpose                            |
| ---------------------- | ------------ | ----- | ---------------------------------- |
| `auction_metrics_pkey` | metric_code  | BTREE | Primary key for fast metric lookup |

---

## Example Row

```json
{
  "metric_code": "yield",
  "description": "Final yield of the auctioned instrument",
  "unit": "%",
  "preferred_format": "0.00%",
  "display_order": 1,
  "created_at": "2025-06-01T10:00:00Z",
  "updated_at": "2025-06-21T09:45:00Z"
}
```

---

## Query Examples

**Get all metrics ordered for UI display:**

```sql
SELECT *
FROM auction_metrics
ORDER BY display_order;
```

**Find metric details by code:**

```sql
SELECT *
FROM auction_metrics
WHERE metric_code = 'yield';
```

---

## Insert Example

```sql
INSERT INTO auction_metrics (metric_code, description, unit, preferred_format, display_order)
VALUES (
  'bid_to_cover',
  'Bid-to-Cover Ratio',
  '',
  '0.00',
  2
);
```