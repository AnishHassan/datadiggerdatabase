# Table: `calendar_event_history`

# Description**

Captures **historical snapshots** of economic calendar events, enabling versioning and analysis of changes over time. Especially useful for tracking revisions in `actual` values and observing how data evolves post-publication.

---

# Schema

| Column Name         | Data Type | Null | Default             | Constraints | Description                                                         |
| ------------------- | --------- | ---- | ------------------- | ----------- | ------------------------------------------------------------------- |
| `id`                | UUID      | NO   | `gen_random_uuid()` | Primary Key | Unique identifier for each historical snapshot                      |
| `calendar_event_id` | UUID      | YES  | NULL                | Foreign Key | References the event from `calendar_events`                         |
| `actual`            | NUMERIC   | YES  | NULL                |             | Value reported at the time this snapshot was recorded               |
| `revision`          | NUMERIC   | YES  | NULL                |             | Adjusted/revised value, if the original `actual` was updated        |
| `recorded_at`       | TIMESTAMP | NO   | `CURRENT_TIMESTAMP` |             | Time the historical record was inserted (used for audit/versioning) |

---

# Relationships

| Related Table     | Relationship Type | Foreign Key         | Description                                                    |
| ----------------- | ----------------- | ------------------- | -------------------------------------------------------------- |
| `calendar_events` | Many-to-One       | `calendar_event_id` | Links the history to the original calendar event being tracked |

---

# Usage Scenarios

* Analyzing **economic forecast accuracy** by comparing original and revised actual values.
* Supporting **versioned datasets** for analytics and modeling.
* Allowing **auditing** of data changes over time.
* Powering **historical visualizations** or event timelines in user interfaces.

---

# Indexes

| Index Name                              | Columns                              | Type  | Description                                                      |
| --------------------------------------- | ------------------------------------ | ----- | ---------------------------------------------------------------- |
| `calendar_event_history_pkey`           | `id`                                 | BTREE | Ensures unique identity of each historical snapshot              |
| `idx_calendar_event_history_event_time` | (`calendar_event_id`, `recorded_at`) | BTREE | Optimizes retrieval of historical records in chronological order |

---

# Example Record

```json
{
  "id": "b923a31c-7c9b-4d91-bcb4-9f8f7d3caa56",
  "calendar_event_id": "45ff5a29-1a6e-47db-8d60-1b13cb521f20",
  "actual": 3.2,
  "revision": 3.1,
  "recorded_at": "2025-06-20T14:00:00"
}
```

---

# Insert Example

```sql
INSERT INTO calendar_event_history (
  id,
  calendar_event_id,
  actual,
  revision,
  recorded_at
)
VALUES (
  gen_random_uuid(),
  '45ff5a29-1a6e-47db-8d60-1b13cb521f20',
  3.2,
  3.1,
  CURRENT_TIMESTAMP
);
```

---

# Query Example

### Get full history for a calendar event (latest first):

```sql
SELECT actual, revision, recorded_at
FROM calendar_event_history
WHERE calendar_event_id = '45ff5a29-1a6e-47db-8d60-1b13cb521f20'
ORDER BY recorded_at DESC;
```