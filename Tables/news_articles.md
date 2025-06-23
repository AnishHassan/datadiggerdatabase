# Table: `news_articles`

---

## Description

Stores news articles ingested from RSS feeds. It includes metadata such as the article’s publishing details, source rankings, categories, and ingestion method. Supports deduplication using hashed URLs and enables efficient filtering, categorization, and delivery tracking.

---

## Schema

| Column Name        | Data Type    | Null | Constraints                            | Description                                                            |
| ------------------ | ------------ | ---- | -------------------------------------- | ---------------------------------------------------------------------- |
| `id`               | INT4         | NO   | Primary Key, Auto-increment            | Unique identifier for each news article                                |
| `feed_id`          | INT4         | YES  | Foreign Key → `rss_feeds.id`           | Optional reference to the RSS feed source                              |
| `headline`         | VARCHAR(500) | YES  | -                                      | Title of the news article                                              |
| `summary`          | TEXT         | YES  | -                                      | Short summary or excerpt of the article                                |
| `article_url`      | VARCHAR(512) | NO   | -                                      | Full URL of the article                                                |
| `published_at`     | TIMESTAMPTZ  | YES  | -                                      | Original publication timestamp                                         |
| `source_shortname` | VARCHAR(50)  | YES  | -                                      | Short code or abbreviation for the source (e.g., "BBC", "CNN")         |
| `world_rank`       | INT4         | YES  | -                                      | Global rank of the article's source                                    |
| `country_rank`     | INT4         | YES  | -                                      | Country-specific rank of the article's source                          |
| `created_at`       | TIMESTAMPTZ  | NO   | Default: `CURRENT_TIMESTAMP`           | Timestamp when the article was recorded                                |
| `updated_at`       | TIMESTAMPTZ  | NO   | Default: `CURRENT_TIMESTAMP`           | Timestamp of the last update to this record                            |
| `category_id`      | INT4         | NO   | Foreign Key → `calendar_categories.id` | ID of the category the article belongs to                              |
| `article_url_hash` | BPCHAR(64)   | NO   | Unique, Default: `''`                  | SHA256 hash of `article_url` used to prevent duplicates                |
| `delivery_method`  | TEXT         | YES  | Default: `'cron'`                      | Ingestion method (e.g., `cron`, `websocket`, etc.)                     |
| `tooltip`          | TEXT         | YES  | -                                      | Optional tooltip or hover description for UI                           |
| `headline_flash`   | VARCHAR(10)  | YES  | Default: `'NO'`                        | Flag for breaking news or attention-worthy article (`'YES'` or `'NO'`) |

---

## Relationships

| Foreign Key   | References               | Description                              |
| ------------- | ------------------------ | ---------------------------------------- |
| `feed_id`     | `rss_feeds.id`           | Links the article to its RSS source feed |
| `category_id` | `calendar_categories.id` | Links the article to a defined category  |

---

## Example Record

```json
{
  "id": 10429,
  "feed_id": 17,
  "headline": "Global Leaders Meet to Discuss AI Regulation",
  "summary": "A global summit today opened discussions on regulating artificial intelligence...",
  "article_url": "https://www.example.com/news/ai-regulation",
  "published_at": "2025-06-22T08:30:00Z",
  "source_shortname": "BBC",
  "world_rank": 102,
  "country_rank": 5,
  "created_at": "2025-06-22T08:30:05Z",
  "updated_at": "2025-06-22T08:30:05Z",
  "category_id": 3,
  "article_url_hash": "ecb3f8a7b3f4c92d6f1e75e92717f5bb32f7f3f01ffb5ad7cf2b93ad8b645d7f",
  "delivery_method": "cron",
  "tooltip": "International regulation discussions kick off",
  "headline_flash": "YES"
}
```

---

## Usage Scenarios

* **Deduplication**: Prevents ingestion of the same article multiple times via `article_url_hash`.
* **Highlighting Flash Articles**: UI can flag or display breaking news using `headline_flash`.
* **Filtering and Categorization**: Supports dynamic filtering by `category_id`, `published_at`, or source ranks.
* **Feed Source Insights**: Enables analysis of content by origin (using `feed_id` or `source_shortname`).
* **Ranking & Trust Evaluation**: Use `world_rank` and `country_rank` for prioritizing or validating sources.
* **Content Delivery Tracking**: Identifies ingestion method (e.g., `cron`, `websocket`) for operational metrics.

---

## Query Examples

### Latest 24 Hours’ Articles

```sql
SELECT headline, published_at
FROM news_articles
WHERE published_at >= NOW() - INTERVAL '1 day'
ORDER BY published_at DESC;
```

### Flash Headlines Only

```sql
SELECT headline, article_url
FROM news_articles
WHERE headline_flash = 'YES'
ORDER BY published_at DESC;
```

### Top Global News Sources This Week

```sql
SELECT source_shortname, MIN(world_rank) AS best_world_rank
FROM news_articles
WHERE published_at >= NOW() - INTERVAL '7 days'
GROUP BY source_shortname
ORDER BY best_world_rank ASC;
```

### Check for Duplicate Articles

```sql
SELECT article_url_hash, COUNT(*) as dup_count
FROM news_articles
GROUP BY article_url_hash
HAVING COUNT(*) > 1;
```

---

## Insert Example

```sql
INSERT INTO news_articles (
  feed_id, headline, summary, article_url, published_at, source_shortname,
  world_rank, country_rank, category_id, article_url_hash, delivery_method,
  tooltip, headline_flash
)
VALUES (
  17,
  'Global Leaders Meet to Discuss AI Regulation',
  'A global summit today opened discussions on regulating artificial intelligence...',
  'https://www.example.com/news/ai-regulation',
  '2025-06-22T08:30:00Z',
  'BBC',
  102,
  5,
  3,
  'ecb3f8a7b3f4c92d6f1e75e92717f5bb32f7f3f01ffb5ad7cf2b93ad8b645d7f',
  'cron',
  'International regulation discussions kick off',
  'YES'
);
``` 