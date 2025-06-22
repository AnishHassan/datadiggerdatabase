# Table: `cron_jobs`

---

# Description

Stores scheduled jobs responsible for triggering RSS feed fetches at defined intervals. Each entry represents an automated task with configurable timing, retry logic, and execution tracking. This allows scalable, timed polling of external sources with failover support.

---

# Schema

| Column Name                   | Data Type   | Null | Constraints                   | Description                                                    |
| ----------------------------- | ----------- | ---- | ----------------------------- | -------------------------------------------------------------- |
| `id`                          | INT8        | NO   | Primary Key, Auto-Increment   | Unique identifier for the scheduled job                        |
| `feed_id`                     | INT4        | NO   | Foreign key to `rss_feeds.id` | Refers to the feed this job will update                        |
| `feed_identifier`             | VARCHAR(50) | NO   | Unique (application-level)    | Redundant identifier matching `rss_feeds.identifier`           |
| `country_iso2`                | BPCHAR(2)   | YES  | ISO 3166-1 alpha-2            | Country code for geo-specific scheduling (optional)            |
| `source_shortname`            | VARCHAR(50) | YES  | -                             | Abbreviated source name (e.g., "NYT", "BBC")                   |
| `cron_expression`             | TEXT        | NO   | -                             | Cron syntax defining the schedule (e.g., `*/15 * * * *`)       |
| `timezone`                    | TEXT        | NO   | Default: `'UTC'`              | Timezone used for evaluating the cron expression               |
| `active`                      | BOOL        | NO   | Default: `true`               | Whether the job is enabled                                     |
| `last_run`                    | TIMESTAMPTZ | NO   | Default: `CURRENT_TIMESTAMP`  | Timestamp of last job execution                                |
| `notes`                       | TEXT        | YES  | -                             | Internal notes or status comments                              |
| `retry_window_seconds`        | INT4        | NO   | Default: `0`                  | Total retry duration window in seconds (0 disables retries)    |
| `retry_interval_seconds`      | INT4        | NO   | Default: `60`                 | Interval between normal retries in seconds                     |
| `max_retries`                 | INT4        | YES  | -                             | Maximum retry attempts (null = unlimited or config-dependent)  |
| `source_priority`             | VARCHAR(20) | YES  | -                             | Priority label (e.g., "high", "medium") for triage or queueing |
| `retry_duration_fast_seconds` | INT4        | YES  | -                             | Optional short retry window for critical feeds                 |
| `retry_interval_fast_seconds` | INT4        | YES  | -                             | Interval between fast retries in seconds                       |

---

# Indexes

| Index Name                      | Columns             | Description                                  |
| ------------------------------- | ------------------- | -------------------------------------------- |
| `idx_cron_jobs_active`          | `(active)`          | Filters by active/inactive status            |
| `idx_cron_jobs_active_only`     | `(feed_identifier)` | Speeds up lookup by feed identifier          |
| `idx_cron_jobs_feed_identifier` | `(feed_identifier)` | Redundant; consider consolidating with above |
| `idx_cron_jobs_last_run`        | `(last_run)`        | Useful for ordering or staleness detection   |


---

# Relationships

* `feed_id` → `rss_feeds.id` (foreign key reference)
* `feed_identifier` ↔ should match `rss_feeds.identifier` (denormalized for performance)
* `country_iso2` may link to `country_subregions.country_iso2` or similar geographic metadata

---

# Example Record

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

# Usage Scenarios

* Automating periodic RSS feed polling.
* Enabling fine-grained control over retry policies for flaky sources.
* Prioritizing critical sources (e.g., emergency news) for faster fallback recovery.
* Monitoring last execution to detect stalled or delayed feeds.

---

# Query Examples

# 1. Find active jobs that haven’t run in the last hour:

```sql
SELECT feed_identifier, last_run
FROM cron_jobs
WHERE active = true
  AND last_run < NOW() - INTERVAL '1 hour';
```

---

# 2. Retrieve jobs scheduled every 15 minutes:

```sql
SELECT id, feed_identifier, cron_expression
FROM cron_jobs
WHERE cron_expression = '*/15 * * * *';
```

---

# 3. List high-priority jobs with fast retry enabled:

```sql
SELECT feed_identifier, retry_duration_fast_seconds, retry_interval_fast_seconds
FROM cron_jobs
WHERE source_priority = 'high'
  AND retry_duration_fast_seconds IS NOT NULL;
```

---

# Insert Example

```sql
INSERT INTO cron_jobs (
  feed_id,
  feed_identifier,
  country_iso2,
  source_shortname,
  cron_expression,
  timezone,
  active,
  retry_window_seconds,
  retry_interval_seconds,
  max_retries,
  source_priority,
  retry_duration_fast_seconds,
  retry_interval_fast_seconds,
  notes
) VALUES (
  17,
  'bbc_world_news',
  'GB',
  'BBC',
  '*/15 * * * *',
  'UTC',
  true,
  0,
  60,
  NULL,
  'high',
  120,
  30,
  'Retry disabled due to upstream throttling.'
);
```