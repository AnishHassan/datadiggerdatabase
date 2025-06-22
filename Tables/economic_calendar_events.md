# Table: `economic_calendar_events`

---

# Description

Stores scheduled economic releases and events (e.g., GDP, CPI, unemployment reports) for global economies. These are critical market-moving events used in economic forecasting, portfolio allocation, and trading strategies.

---

# Schema

| Column Name        | Data Type    | Null | Default  | Constraints | Description |
| ------------------ | ------------ | ---- | -------------------    | -------------------------------------------------------------------------------------- |
| `id`               | INT4         | NO   | -        | Primary Key, Auto-Incr    | Unique identifier for the economic event                                 |
| `indicator_code`   | VARCHAR(32)  | YES  | NULL     | -                         | Code representing the economic indicator (e.g., `GDP_QOQ`, `CPI`)        |
| `country_iso2`     | BPCHAR(2)    | YES  | NULL     | Foreign Key → `countries` | ISO 3166-1 alpha-2 country code (e.g., `US`, `JP`, `DE`)                 |
| `source_shortname` | VARCHAR(32)  | YES  | NULL     | -                         | Short name of the source or publisher (e.g., `BLS`, `Eurostat`, `BEA`)   |
| `region`           | VARCHAR(64)  | YES  | NULL     | -                         | Economic region or bloc (e.g., `Eurozone`, `Asia-Pacific`)               |
| `date`             | DATE         | NO   | -        | -                         | Scheduled date of the event                                              |
| `release_for`      | DATE         | YES  | NULL     | -                         | The data period the release refers to (e.g., end of Q1)                  |
| `time`             | TIME         | YES  | NULL     | -                         | Scheduled time of the release (may depend on `timezone`)                 |
| `timezone`         | VARCHAR(64)  | YES  | `'UTC'`  | -                         | Timezone identifier (e.g., `UTC`, `America/New_York`)                    |
| `title`            | VARCHAR(255) | YES  | NULL     | -                         | Human-readable title of the release or event                             |
| `impact`           | VARCHAR(6)   | YES  | `'Low'`  | CHECK (`impact` IN ...)   | Subjective impact level: `Low`, `Medium`, or `High`                      |
| `parent_event_id`  | INT4         | YES  | NULL     | FK → self (`id`)          | Links to parent event (for revisions, grouping, or composite indicators) |
| `created_at`       | TIMESTAMP    | YES  | `CURRENT_TIMESTAMP` | -              | Record creation timestamp                                                |

---

# Index & Constraints Highlights

* Implicit primary index on `id`
* Expected FK to `countries.country_iso2` (geopolitical joins)
* Expected self-referencing FK on `parent_event_id` for hierarchical grouping

---

# Relationships

| Related Table              | Column            | Relationship     | Description                                      |
| -------------------------- | ----------------- | ---------------- | ------------------------------------------------ |
| `countries`                | `country_iso2`    | Many-to-One      | Joins with countries for full metadata           |
| `economic_calendar_events` | `parent_event_id` | Self-referencing | Enables hierarchical/revision tracking of events |

---

# Example Record

```json
{
  "id": 12453,
  "indicator_code": "GDP_QOQ",
  "country_iso2": "US",
  "source_shortname": "BEA",
  "region": "North America",
  "date": "2025-07-30",
  "release_for": "2025-06-30",
  "time": "08:30:00",
  "timezone": "America/New_York",
  "title": "Gross Domestic Product (QoQ) Final",
  "impact": "High",
  "parent_event_id": null,
  "created_at": "2025-06-21 14:22:11"
}
```

---

# Usage Scenarios

* Driving economic newsfeeds and market calendars.
* Triggering user alerts or notifications based on `impact` or `indicator_code`.
* Grouping preliminary, revised, and final versions using `parent_event_id`.
* Supporting real-time dashboards or timelines segmented by country or region.

---

# Query Examples

# High-impact U.S. events in July 2025

```sql
SELECT title, date, time, impact
FROM economic_calendar_events
WHERE country_iso2 = 'US'
  AND impact = 'High'
  AND date BETWEEN '2025-07-01' AND '2025-07-31'
ORDER BY date, time;
```

---

# Join with country names

```sql
SELECT e.title, e.date, c.country_long_name
FROM economic_calendar_events e
JOIN countries c ON e.country_iso2 = c.country_iso2
WHERE e.impact = 'Medium'
ORDER BY e.date;
```

---

# Find revised/final versions of an event (grouped by parent)

```sql
SELECT *
FROM economic_calendar_events
WHERE parent_event_id = 12453
ORDER BY date, time;
```

---

# Insert Example

```sql
INSERT INTO economic_calendar_events (
  indicator_code,
  country_iso2,
  source_shortname,
  region,
  date,
  release_for,
  time,
  timezone,
  title,
  impact,
  parent_event_id
) VALUES (
  'CPI_YOY',
  'US',
  'BLS',
  'North America',
  '2025-08-14',
  '2025-07-31',
  '08:30:00',
  'America/New_York',
  'Consumer Price Index (YoY) Final',
  'High',
  NULL
);
```