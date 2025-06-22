# Table: `rss_feeds`

### **Description**

Represents RSS feed sources and their configuration metadata. Each record defines the origin, category, scheduling, and filtering rules for an individual feed.

---

## Schema

| Column Name            | Data Type    | Null | Constraints                  | Description                                                    |
| ---------------------- | ------------ | ---- | ---------------------------- | -------------------------------------------------------------- |
| `id`                   | INT4         | NO   | Primary Key, Auto-Increment  | Unique feed ID                                                 |
| `identifier`           | VARCHAR(50)  | NO   | Unique                       | System-wide unique identifier for the feed                     |
| `source_name`          | VARCHAR(255) | NO   | -                            | Original source or organization name                           |
| `feed_name`            | VARCHAR(255) | NO   | -                            | Display name of the feed                                       |
| `feed_url`             | TEXT         | NO   | -                            | Full URL to fetch the feed from                                |
| `active`               | BOOL         | YES  | Default: `true`              | Whether the feed is currently active and processed             |
| `last_fetched`         | TIMESTAMP    | YES  | -                            | Timestamp of last successful fetch                             |
| `source_published_at`  | TIMESTAMP    | YES  | -                            | Timestamp of original publish from the source                  |
| `created_at`           | TIMESTAMP    | YES  | Default: `CURRENT_TIMESTAMP` | Record creation time                                           |
| `updated_at`           | TIMESTAMP    | YES  | Default: `CURRENT_TIMESTAMP` | Record last update time                                        |
| `source_longname`      | VARCHAR(255) | YES  | -                            | Longer human-readable name for the source                      |
| `source_shortname`     | VARCHAR(50)  | YES  | -                            | Abbreviated source name (e.g., “BBG” for Bloomberg)            |
| `country_short_name`   | VARCHAR(100) | YES  | -                            | Country name in readable form                                  |
| `country_iso2`         | BPCHAR(2)    | YES  | -                            | ISO 3166-1 alpha-2 country code                                |
| `language_code`        | BPCHAR(2)    | YES  | -                            | Language code (linked to `languages.language_code`)            |
| `category_id`          | INT4         | YES  | -                            | Feed classification/category ID                                |
| `wire_group_id`        | INT4         | YES  | -                            | Associated wire group (from `wire_groups`)                     |
| `tags`                 | VARCHAR(255) | YES  | -                            | Optional keyword tags for filtering/grouping                   |
| `update_method`        | TEXT         | YES  | Default: `'websocket'`       | Method used for updating (e.g., webhook, polling, websocket)   |
| `seasonal_winter_only` | BOOL         | YES  | Default: `false`             | Indicates if the feed is active only during winter seasons     |
| `filter_path`          | VARCHAR(255) | YES  | -                            | Path or key used for internal filtering of incoming feed items |

---

## Relationships (Expected/Optional)

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
  "tags": "breaking, world",
  "update_method": "websocket",
  "seasonal_winter_only": false,
  "filter_path": "channel.item"
}
```

---

## Usage Scenarios

* Configuring and ingesting multiple RSS feed sources dynamically.
* Managing feed-specific behavior like filtering, language handling, or scheduling.
* Grouping feeds by country, wire group, or category.

---

## Query Examples

### Get all active RSS feeds by country:

```sql
SELECT feed_name, source_name, feed_url
FROM rss_feeds
WHERE active = true AND country_iso2 = 'US';
```

### Feeds not updated in the last 24 hours:

```sql
SELECT id, feed_name, last_fetched
FROM rss_feeds
WHERE last_fetched < NOW() - INTERVAL '1 day';
```