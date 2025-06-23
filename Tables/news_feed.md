# Table: `news_feed`

---

## Description

Captures short-form news entries, alerts, and bulletins—often more digestible than full articles. Includes essential metadata such as publish date/time, source, region, and trending status to support timely and region-specific delivery.

---

## Schema

| Column Name    | Data Type | Null | Default             | Constraints                     | Description                                      |
| -------------- | --------- | ---- | ------------------- | ------------------------------- | ------------------------------------------------ |
| `id`           | UUID      | NO   | `gen_random_uuid()` | Primary Key                     | Unique identifier for the news feed entry        |
| `publish_date` | DATE      | NO   | -                   |                                 | The date the news feed was published             |
| `publish_time` | TIME      | NO   | -                   |                                 | The time the news feed was published             |
| `headline`     | TEXT      | NO   | -                   |                                 | Title or main line of the news                   |
| `summary`      | TEXT      | YES  | NULL                |                                 | Optional short summary of the news content       |
| `link`         | TEXT      | YES  | NULL                |                                 | Optional external link to full content or source |
| `is_trending`  | BOOLEAN   | YES  | `false`             |                                 | Marks whether the item is currently trending     |
| `region_id`    | INT       | YES  | NULL                | Foreign Key → `regions.id`      | Optionally links the news to a region            |
| `source_id`    | INT       | YES  | NULL                | Foreign Key → `news_sources.id` | Links to the original source of the news item    |
| `created_at`   | TIMESTAMP | YES  | `CURRENT_TIMESTAMP` |                                 | Timestamp when the record was created            |

---

## Relationships

| Foreign Key | References        | Relationship | Description                                          |
| ----------- | ----------------- | ------------ | ---------------------------------------------------- |
| `region_id` | `regions.id`      | Many-to-One  | Optionally associates a news feed item with a region |
| `source_id` | `news_sources.id` | Many-to-One  | Identifies the original source of the news item      |

---

## Example Record

```json
{
  "id": "c1f7d9a2-3df8-4b20-84b4-96c8dc823e02",
  "publish_date": "2025-06-21",
  "publish_time": "09:30:00",
  "headline": "Global Markets Rise Ahead of Fed Meeting",
  "summary": "Stocks climb as investors anticipate signals on future interest rates.",
  "link": "https://example.com/markets-rise-fed",
  "is_trending": true,
  "region_id": 1,
  "source_id": 2,
  "created_at": "2025-06-21T09:30:00Z"
}
```

---

## Usage Scenarios

* **Display Digest News**: Serve short-format bulletins in widgets or mobile notifications.
* **Trend Analysis**: Track what’s trending regionally or globally via the `is_trending` flag.
* **Region-Based Filtering**: Filter news feeds by geographical region using `region_id`.
* **Source Validation**: Track or analyze news credibility via linked `source_id`.
* **Chronological Feeds**: Sort/display news chronologically using `publish_date` and `publish_time`.

---

## Indexes

| Index Name                      | Columns                        | Type  | Purpose                                        |
| ------------------------------- | ------------------------------ | ----- | ---------------------------------------------- |
| `news_feed_pkey`                | `id`                           | BTREE | Ensures unique identification                  |
| `idx_publish_date_publish_time` | `(publish_date, publish_time)` | BTREE | Optimizes chronological ordering and filtering |

---

## Query Examples

### Get Latest Trending News (Top 5)

```sql
SELECT headline, summary, publish_date, publish_time
FROM news_feed
WHERE is_trending = true
ORDER BY publish_date DESC, publish_time DESC
LIMIT 5;
```

### Region-Specific News in Last 48 Hours

```sql
SELECT *
FROM news_feed
WHERE region_id = 1
  AND publish_date >= CURRENT_DATE - INTERVAL '2 days'
ORDER BY publish_date DESC, publish_time DESC;
```

### News with External Links

```sql
SELECT headline, link
FROM news_feed
WHERE link IS NOT NULL
ORDER BY publish_date DESC, publish_time DESC;
```

---

## Insert Example

```sql
INSERT INTO news_feed (
  id, publish_date, publish_time, headline, summary, link,
  is_trending, region_id, source_id, created_at
)
VALUES (
  gen_random_uuid(),
  '2025-06-21',
  '09:30:00',
  'Global Markets Rise Ahead of Fed Meeting',
  'Stocks climb as investors anticipate signals on future interest rates.',
  'https://example.com/markets-rise-fed',
  true,
  1,
  2,
  CURRENT_TIMESTAMP
);
```