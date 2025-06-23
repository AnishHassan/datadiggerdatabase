# Table: `rss_feeds`

### **Description**

Represents individual RSS feed sources along with their configuration, scheduling behavior, and metadata. Each record defines where the feed originates from, what category it belongs to, how it should be updated, and any filtering criteria applied during ingestion.

---

## Schema

| Column Name            | Data Type    | Null | Constraints               | Description                                                                    |
| -------------------    | ------------ | ---- | ------------------------- | ------------------------------------------------------------------------------ |
| `id`                   | INT4         | NO | Primary Key, Auto-Increment | Unique feed ID                                                                    |
| `identifier`           | VARCHAR(50)  | NO | Unique                      | System-wide unique identifier (used internally)                                   |
| `source_name`          | VARCHAR(255) | NO | -                           | Name of the feed's original content provider                                      |
| `feed_name`            | VARCHAR(255) | NO | -                           | Display-friendly name of the RSS feed                                             |
| `feed_url`             | TEXT         | NO | -                           | Full URL used to fetch the RSS feed                                               |
| `active`               | BOOL         | YES| Default: `true`             | Indicates whether this feed is currently active                                   |
| `last_fetched`         | TIMESTAMP    | YES| -                           | Timestamp of the most recent successful fetch                                     |
| `source_published_at`  | TIMESTAMP    | YES| -                           | Timestamp of the last known article from the source                               |
| `created_at`           | TIMESTAMP    | YES| Default: `CURRENT_TIMESTAMP`| Time when the record was created                                                  |
| `updated_at`           | TIMESTAMP    | YES| Default: `CURRENT_TIMESTAMP`| Time when the record was last modified                                            |
| `source_longname`      | VARCHAR(255) | YES| -                           | Full human-readable name of the source (e.g., "British Broadcasting Corporation") |
| `source_shortname`     | VARCHAR(50)  | YES| -                           | Abbreviated or short label (e.g., "BBC", "BBG")                                   |
| `country_short_name`   | VARCHAR(100) | YES| -                           | Country name in a human-readable format                                           |
| `country_iso2`         | BPCHAR(2)    | YES| -                           | ISO 3166-1 alpha-2 country code                                                   |
| `language_code`        | BPCHAR(2)    | YES| -                           | Language code (FK to `languages.language_code`)                                   |
| `category_id`          | INT4         | YES| -                           | Foreign key to category (`calendar_categories.id`)                                |
| `wire_group_id`        | INT4         | YES| -                           | Foreign key to groupings (`wire_groups.id`)                                       |
| `tags`                 | VARCHAR(255) | YES| -                           | Comma-separated string of freeform tags                                           |
| `update_method`        | TEXT         | YES| Default: `'websocket'`      | Feed update mechanism: `webhook`, `polling`, `websocket`, etc.                    |
| `seasonal_winter_only` | BOOL         | YES| Default: `false`            | Indicates whether the feed is active only in winter months                        |
| `filter_path`          | VARCHAR(255) | YES| -                           | Internal path for selective filtering of feed content                             |

---

## Relationships

* `language_code` → **`languages.language_code`**
* `category_id` → **`calendar_categories.id`**
* `wire_group_id` → **`wire_groups.id`**

---

## Example Record

```json
{
  "id": 17,
  "identifier": "bbc_world_news",
  "source_name": "BBC",
  "feed_name": "BBC World News",
  "feed_url": "https://feeds.bbci.co.uk/news/world/rss.xml",
  "active": true,
  "last_fetched": "2025-06-21T12:30:00",
  "source_published_at": "2025-06-21T11:55:00",
  "created_at": "2024-03-01T08:00:00",
  "updated_at": "2025-06-21T12:30:00",
  "source_longname": "British Broadcasting Corporation",
  "source_shortname": "BBC",
  "country_short_name": "United Kingdom",
  "country_iso2": "GB",
  "language_code": "en",
  "category_id": 2,
  "wire_group_id": 1,
  "tags": "breaking,world",
  "update_method": "websocket",
  "seasonal_winter_only": false,
  "filter_path": "channel.item"
}
```

---

## Usage Scenarios

* Dynamically configure multiple RSS feeds for ingestion and management.
* Filter and categorize feeds based on region, language, seasonality, or topic.
* Drive logic for automated feed fetching and UI categorization.
* Use `update_method` to determine push vs pull models for real-time updates.

---

## Query Examples

### 1. Get all active RSS feeds by country:

```sql
SELECT feed_name, source_name, feed_url
FROM rss_feeds
WHERE active = true AND country_iso2 = 'US';
```

### 2. Feeds not updated in the last 24 hours:

```sql
SELECT id, feed_name, last_fetched
FROM rss_feeds
WHERE last_fetched < NOW() - INTERVAL '1 day';
```

### 3. Count of feeds by update method:

```sql
SELECT update_method, COUNT(*) AS feed_count
FROM rss_feeds
GROUP BY update_method;
```

### 4. Fetch all inactive feeds:

```sql
SELECT id, identifier, feed_name
FROM rss_feeds
WHERE active = false;
```

---

## Insert Example

```sql
INSERT INTO rss_feeds (
  identifier, source_name, feed_name, feed_url, active,
  source_longname, source_shortname, country_short_name, country_iso2,
  language_code, category_id, wire_group_id, tags, update_method,
  seasonal_winter_only, filter_path
)
VALUES (
  'nyt_global', 'New York Times', 'NYT Global Headlines', 'https://rss.nytimes.com/services/xml/rss/nyt/World.xml',
  true, 'The New York Times', 'NYT', 'United States', 'US',
  'en', 3, 2, 'global,politics', 'polling', false, 'rss.channel.item'
);
```