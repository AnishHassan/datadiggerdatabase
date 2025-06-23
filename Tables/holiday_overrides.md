# Table: `holiday_overrides`

---

## **Description**

Defines **manual overrides** to the standard release schedule of economic data feeds when impacted by holidays or special circumstances. It allows explicitly setting new release dates and times, ensuring accurate, holiday-aware data availability.

---

## **Schema**

| Column Name          | Data Type    | Null | Default             | Constraints | Description                                                       |
| -------------------- | ------------ | ---- | ------------------- | ----------- | ----------------------------------------------------------------- |
| `id`                 | INT4         | NO   | Auto-Increment      | Primary Key | Unique identifier for each override record                        |
| `feed_id`            | INT4         | NO   | –                   |             | Identifier of the affected data feed (not a declared foreign key) |
| `release_date`       | DATE         | NO   | –                   |             | New release date overriding the default                           |
| `release_time_local` | TIME         | YES  | `'13:00:00'`        |             | New local release time (optional; defaults to early afternoon)    |
| `cron_expression`    | VARCHAR(255) | YES  | `NULL`              |             | Optional CRON syntax for custom scheduling logic                  |
| `holiday_name`       | VARCHAR(255) | YES  | `NULL`              |             | Name of the holiday or reason for the manual override             |
| `created_at`         | TIMESTAMP    | YES  | `CURRENT_TIMESTAMP` |             | Timestamp marking when the override was logged                    |

---

## **Business Rules**

* Overrides should be added **only when default scheduling is disrupted**.
* `feed_id` must correspond to a valid internal or external feed system.
* `release_date` is mandatory; `release_time_local` is optional.
* `cron_expression` can be used to describe recurring patterns (e.g., if a holiday occurs every year).

---

## **Indexes**

| Index Name               | Column(s)        | Type  | Description                                |
| ------------------------ | ---------------- | ----- | ------------------------------------------ |
| `holiday_overrides_pkey` | `id`             | BTREE | Primary key for fast lookup by ID          |
| *(Optional suggested)*   | `(feed_id)`      | BTREE | To optimize queries filtering by feed      |
| *(Optional suggested)*   | `(release_date)` | BTREE | To accelerate lookup of upcoming overrides |

---

## **Example Record**

```json
{
  "id": 17,
  "feed_id": 201,
  "release_date": "2025-12-25",
  "release_time_local": "10:00:00",
  "cron_expression": null,
  "holiday_name": "Christmas Day",
  "created_at": "2025-11-15T08:30:00"
}
```

---

## **Usage Scenarios**

* Automatically rescheduling economic data feeds to non-business days.
* Integrating exception calendars into APIs or dashboards.
* Enabling internal QA or operations teams to track data timing anomalies.

---

## **Query Examples**

### Get all upcoming overrides

```sql
SELECT * 
FROM holiday_overrides
WHERE release_date >= CURRENT_DATE
ORDER BY release_date;
```

---

### Find overrides for a specific feed

```sql
SELECT *
FROM holiday_overrides
WHERE feed_id = 201
ORDER BY release_date DESC;
```

---

### Count of overrides per feed

```sql
SELECT feed_id, COUNT(*) AS override_count
FROM holiday_overrides
GROUP BY feed_id
ORDER BY override_count DESC;
```