# Table: `cron_jobs`

### **Description**

Manages scheduling and retry logic for RSS feed updates. Each record defines a job that periodically triggers a fetch process for a specific feed.

---

## Schema

| Column Name                   | Data Type   | Null | Constraints                  | Description                                                    |
| ----------------------------- | ----------- | ---- | ---------------------------- | -------------------------------------------------------------- |
| `id`                          | INT8        | NO   | Primary Key, Auto-Increment  | Unique job ID                                                  |
| `feed_id`                     | INT4        | NO   | -                            | Foreign key to `rss_feeds.id`                                  |
| `feed_identifier`             | VARCHAR(50) | NO   | -                            | System-wide unique identifier matching `rss_feeds.identifier`  |
| `country_iso2`                | BPCHAR(2)   | YES  | -                            | ISO 3166-1 alpha-2 country code                                |
| `source_shortname`            | VARCHAR(50) | YES  | -                            | Abbreviated source name (e.g., "NYT", "BBC")                   |
| `cron_expression`             | TEXT        | NO   | -                            | Cron string specifying job schedule                            |
| `timezone`                    | TEXT        | NO   | Default: `'UTC'`             | Timezone used for cron interpretation                          |
| `active`                      | BOOL        | NO   | Default: `true`              | Whether the job is active and scheduled                        |
| `last_run`                    | TIMESTAMPTZ | NO   | Default: `CURRENT_TIMESTAMP` | Last execution timestamp                                       |
| `notes`                       | TEXT        | YES  | -                            | Developer/admin notes for this job                             |
| `retry_window_seconds`        | INT4        | NO   | Default: `0`                 | Total retry window in seconds (0 = no retries)                 |
| `retry_interval_seconds`      | INT4        | NO   | Default: `60`                | Interval between retries (seconds)                             |
| `max_retries`                 | INT4        | YES  | -                            | Max number of retry attempts                                   |
| `source_priority`             | VARCHAR(20) | YES  | -                            | Optional priority tag (e.g., "high", "medium")                 |
| `retry_duration_fast_seconds` | INT4        | YES  | -                            | Optional short retry window (e.g., for high-priority failures) |
| `retry_interval_fast_seconds` | INT4        | YES  | -                            | Interval between fast retries                                  |

---

## Indexes

| Index Name                      | Columns             | Description                                |
| ------------------------------- | ------------------- | ------------------------------------------ |
| `idx_cron_jobs_active`          | `(active)`          | Filter for active jobs                     |
| `idx_cron_jobs_active_only`     | `(feed_identifier)` | Fast lookup by identifier                  |
| `idx_cron_jobs_feed_identifier` | `(feed_identifier)` | Another identifier index (possibly legacy) |
| `idx_cron_jobs_last_run`        | `(last_run)`        | Sorting/filtering by last execution time   |

> 🔎 **Note**: There is a duplicate index on `feed_identifier`. You may want to consolidate or rename for clarity.

---

## Relationships (Expected)

* `feed_id` → **`rss_feeds.id`**
* `feed_identifier` ↔ should match `rss_feeds.identifier` (redundant for denormalized speed)

---

## Example Record

```json
{
  "id": 235,
  "feed_id": 17,
  "feed_identifier": "bbc_world_news",
  "country_iso2": "GB",
  "source_shortname": "BBC",
  "cron_expression": "*/15 * * * *",
  "timezone": "UTC",
  "active": true,
  "last_run": "2025-06-22T04:45:00Z",
  "notes": "Retry disabled due to upstream throttling.",
  "retry_window_seconds": 0,
  "retry_interval_seconds": 60,
  "max_retries": null,
  "source_priority": "high",
  "retry_duration_fast_seconds": 120,
  "retry_interval_fast_seconds": 30
}
```

---

## Usage Scenarios

* Periodic job scheduling using cron expressions.
* Dynamic retry mechanisms for failing feed fetches.
* Priority handling for high-value or time-sensitive feeds.

---

## Query Examples

### All active jobs that failed to run in the last hour:

```sql
SELECT feed_identifier, last_run
FROM cron_jobs
WHERE active = true
  AND last_run < NOW() - INTERVAL '1 hour';
```

### Jobs scheduled every 15 minutes:

```sql
SELECT id, feed_identifier, cron_expression
FROM cron_jobs
WHERE cron_expression = '*/15 * * * *';
```