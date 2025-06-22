# Table: `calendar_events`

# Description

Represents **scheduled economic or market-related events** tied to a specific date, time, and region. Each event records indicators such as **survey estimates**, **actual outcomes**, **prior values**, and **revisions**, while also being classified by **region**, **category**, and **industry** for analytical use cases.

---

# Schema

| Column Name   | Data Type | Null | Default             | Constraints | Description                                                         |
| ------------- | --------- | ---- | ------------------- | ----------- | ------------------------------------------------------------------- |
| `id`          | UUID      | NO   | `gen_random_uuid()` | Primary Key | Unique identifier for the calendar event                            |
| `event_date`  | DATE      | NO   | -                   |             | Date on which the event is scheduled                                |
| `event_time`  | TIME      | NO   | -                   |             | Exact time the event is set to occur                                |
| `event_name`  | TEXT      | NO   | -                   |             | Title or short description of the event                             |
| `region_id`   | INT       | YES  | NULL                | Foreign Key | Links to the `regions` table, indicating the geographical scope     |
| `period`      | TEXT      | YES  | NULL                |             | Reporting period (e.g., "Q1", "April") the event data pertains to   |
| `survey`      | NUMERIC   | YES  | NULL                |             | Forecasted/expected value before the event                          |
| `actual`      | NUMERIC   | YES  | NULL                |             | Reported or observed value from the event                           |
| `prior`       | NUMERIC   | YES  | NULL                |             | Value reported in the previous release                              |
| `revision`    | NUMERIC   | YES  | NULL                |             | Updated version of the `prior` value if revised                     |
| `importance`  | TEXT      | YES  | NULL                |             | Importance level for prioritization (e.g., "low", "medium", "high") |
| `category_id` | INT       | YES  | NULL                | Foreign Key | Links to `categories` table for event classification                |
| `industry_id` | INT       | YES  | NULL                | Foreign Key | Links to `industries` table for sectoral relevance                  |
| `updated_at`  | TIMESTAMP | YES  | `CURRENT_TIMESTAMP` |             | Tracks when the event record was last updated                       |

---

# Relationships

| Related Table | Relationship Type | Foreign Key   | Description                                                      |
| ------------- | ----------------- | ------------- | ---------------------------------------------------------------- |
| `regions`     | Many-to-One       | `region_id`   | Identifies the region or country associated with the event       |
| `categories`  | Many-to-One       | `category_id` | Associates the event with a thematic category (e.g., employment) |
| `industries`  | Many-to-One       | `industry_id` | Classifies the event by affected industry (e.g., manufacturing)  |

---

# Business Rules

* Events **must have a valid `event_date` and `event_time`** for accurate scheduling.
* The `survey`, `actual`, `prior`, and `revision` fields can all be `NULL` if unavailable at the time of entry.
* `importance` is typically used to filter/highlight events on dashboards.
* `updated_at` allows tracking of when values are modified — useful for caching, alerts, and audit trails.

---

# Indexes

| Index Name                   | Columns      | Type  | Description                                     |
| ---------------------------- | ------------ | ----- | ----------------------------------------------- |
| `calendar_events_pkey`       | `id`         | BTREE | Primary key for fast lookups by event ID        |
| `calendar_events_event_date` | `event_date` | BTREE | Optimizes range queries on upcoming/past events |

---

# Example Record

```json
{
  "id": "a8f7c5d2-6c3e-4f5d-b431-893a2c8d9981",
  "event_date": "2025-07-01",
  "event_time": "14:30:00",
  "event_name": "US Non-Farm Payrolls",
  "region_id": 1,
  "period": "June",
  "survey": 250000,
  "actual": 270000,
  "prior": 245000,
  "revision": 248000,
  "importance": "high",
  "category_id": 3,
  "industry_id": 5,
  "updated_at": "2025-06-21T10:30:00Z"
}
```

---

# Insert Example

```sql
INSERT INTO calendar_events (
  id, event_date, event_time, event_name, region_id, period,
  survey, actual, prior, revision, importance,
  category_id, industry_id, updated_at
)
VALUES (
  gen_random_uuid(),
  '2025-07-01',
  '14:30:00',
  'US Non-Farm Payrolls',
  1,
  'June',
  250000,
  270000,
  245000,
  248000,
  'high',
  3,
  5,
  CURRENT_TIMESTAMP
);
```

---

# Query Example

### Find high-importance events for a given date range:

```sql
SELECT event_name, event_date, event_time, actual, prior
FROM calendar_events
WHERE importance = 'high'
  AND event_date BETWEEN '2025-07-01' AND '2025-07-07'
ORDER BY event_date, event_time;
```
