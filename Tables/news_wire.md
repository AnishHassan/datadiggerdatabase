# Table: `news_wire`

---

## Description

Stores real-time news entries relevant to financial markets, economic policy, and global/regional events. Each entry captures precise timestamped metadata, source attribution, topical classification, and optional contextual linking to events and regions.

---

## Schema

| Column Name        | Data Type | Null | Default             | Constraints | Description                                                           |
| ------------------ | --------- | ---- | ------------------- | ----------- | --------------------------------------------------------------------- |
| `id`               | UUID      | NO   | `gen_random_uuid()` | Primary Key | Unique identifier for the news item                                   |
| `news_date`        | DATE      | NO   | -                   |             | Calendar date when the news was reported                              |
| `news_time`        | TIME      | NO   | -                   |             | Exact time (HH\:MM\:SS) of news publication                           |
| `headline`         | TEXT      | NO   | -                   |             | Main headline or title of the news                                    |
| `news_type`        | TEXT      | NO   | -                   |             | News category (e.g., `macro`, `policy`, `breaking`)                   |
| `source`           | TEXT      | NO   | -                   |             | Name of the originating news provider (e.g., Reuters, Bloomberg)      |
| `source_logo`      | TEXT      | YES  | NULL                |             | Optional logo URL for the news source                                 |
| `region_id`        | INT       | YES  | NULL                | Foreign Key | Optional reference to a geographic region from the `regions` table    |
| `link`             | TEXT      | YES  | NULL                |             | Optional external link to the original news source                    |
| `related_event_id` | UUID      | YES  | NULL                | Foreign Key | Optional link to an economic or calendar event (`calendar_events.id`) |

---

## Relationships

| Referenced Table  | Foreign Key        | Type        | Description                                             |
| ----------------- | ------------------ | ----------- | ------------------------------------------------------- |
| `regions`         | `region_id`        | Many-to-One | Associates news items with a specific geographic region |
| `calendar_events` | `related_event_id` | Many-to-One | Optionally ties news to an economic or calendar event   |

---

## Business Rules

* News **must** include a `headline`, `news_type`, `source`, `news_date`, and `news_time`.
* If `region_id` or `related_event_id` are specified, they must correspond to existing entries.
* News is typically displayed or filtered in reverse chronological order (latest first).
* The combination of `news_date` and `news_time` determines the true publication timestamp.

---

## Indexes

| Index Name                | Columns                    | Type  | Description                                   |
| ------------------------- | -------------------------- | ----- | --------------------------------------------- |
| `news_wire_pkey`          | `id`                       | BTREE | Ensures uniqueness of each news item          |
| `idx_news_wire_date_time` | (`news_date`, `news_time`) | BTREE | Optimizes chronological filtering and sorting |

---

## Example Record

```json
{
  "id": "ae2b340e-d81d-4f87-94b3-b1f05e8971d4",
  "news_date": "2025-07-01",
  "news_time": "13:45:00",
  "headline": "ECB Hints at Rate Hike Amid Inflation Pressure",
  "news_type": "monetary_policy",
  "source": "Reuters",
  "source_logo": "https://example.com/logos/reuters.png",
  "region_id": 2,
  "link": "https://reuters.com/article/ecb-policy-news",
  "related_event_id": "f1a6d8c2-4fdc-4921-a9e4-5a1c9cf251aa"
}
```

---

## Usage Scenarios

* **Streaming news feeds** for financial dashboards or trading terminals.
* **Filtering news** by region, topic, or related economic event.
* **Attributing stories** clearly with brand logos and publisher links.
* **Linking** macroeconomic events (e.g., central bank decisions) to real-time coverage.

---

## Query Examples

### 🔹 Latest News

```sql
SELECT * FROM news_wire
ORDER BY news_date DESC, news_time DESC
LIMIT 10;
```

### 🔹 News for a Specific Region

```sql
SELECT * FROM news_wire
WHERE region_id = 3
ORDER BY news_date DESC, news_time DESC;
```

### 🔹 News Related to a Calendar Event

```sql
SELECT * FROM news_wire
WHERE related_event_id = 'f1a6d8c2-4fdc-4921-a9e4-5a1c9cf251aa';
```