# Table: `news_articles`

### **Description**

Stores news articles ingested from RSS feeds. Includes metadata such as publishing info, feed source, categories, and rankings. Supports both deduplication (via hash) and delivery tracking.

---

## Schema

| Column Name        | Data Type    | Null | Constraints                     | Description                                                             |
| ------------------ | ------------ | ---- | ------------------------------- | ----------------------------------------------------------------------- |
| `id`               | INT4         | NO   | PK, Auto-increment              | Unique article ID                                                       |
| `feed_id`          | INT4         | YES  | FK to `rss_feeds.id` (optional) | Source RSS feed reference                                               |
| `headline`         | VARCHAR(500) | YES  | -                               | Title of the article                                                    |
| `summary`          | TEXT         | YES  | -                               | Short summary or excerpt                                                |
| `article_url`      | VARCHAR(512) | NO   | -                               | Full URL to the news article                                            |
| `published_at`     | TIMESTAMPTZ  | YES  | -                               | Timestamp when article was originally published                         |
| `source_shortname` | VARCHAR(50)  | YES  | -                               | Short code for the news source (e.g., "BBC", "CNN")                     |
| `world_rank`       | INT4         | YES  | -                               | Global rank of the source (e.g., Alexa or SimilarWeb)                   |
| `country_rank`     | INT4         | YES  | -                               | Country-specific source ranking                                         |
| `created_at`       | TIMESTAMPTZ  | NO   | Default: `CURRENT_TIMESTAMP`    | Record creation timestamp                                               |
| `updated_at`       | TIMESTAMPTZ  | NO   | Default: `CURRENT_TIMESTAMP`    | Last updated timestamp                                                  |
| `category_id`      | INT4         | NO   | FK to `calendar_categories.id`? | Associated category ID                                                  |
| `article_url_hash` | BPCHAR(64)   | NO   | Unique, Default: `''`           | SHA256 hash of `article_url` for deduplication                          |
| `delivery_method`  | TEXT         | YES  | Default: `'cron'`               | Source ingestion method: e.g., `cron`, `websocket`                      |
| `tooltip`          | TEXT         | YES  | -                               | Optional tooltip or hover description                                   |
| `headline_flash`   | VARCHAR(10)  | YES  | Default: `'NO'`                 | Flag indicating breaking or highlighted article (e.g., `'YES'`, `'NO'`) |

---

## Indexes

| Index Name                       | Columns            | Description                       |
| -------------------------------- | ------------------ | --------------------------------- |
| `idx_news_articles_category_id`  | `category_id`      | Lookup/filter by category         |
| `idx_news_articles_feed_id`      | `feed_id`          | Lookup/filter by feed             |
| `idx_news_articles_published_at` | `published_at`     | Sorting/filtering by publish time |
| `UNIQUE`                         | `article_url_hash` | Prevents duplicate articles       |

---

## Relationships (Assumed)

* `feed_id` → `rss_feeds.id`
* `category_id` → `calendar_categories.id` (likely)

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

* RSS feed aggregation
* Content filtering by category, region, or publication time
* De-duplication via URL hash
* Highlighting flash headlines for UI display
* Article performance or rank-based prioritization

---

## Query Examples

### Get the latest articles published in the past 24 hours

```sql
SELECT headline, published_at
FROM news_articles
WHERE published_at >= NOW() - INTERVAL '1 day'
ORDER BY published_at DESC;
```

### Check for duplicate articles

```sql
SELECT article_url, COUNT(*)
FROM news_articles
GROUP BY article_url
HAVING COUNT(*) > 1;
```

### Top sources by world rank in the past week

```sql
SELECT source_shortname, MIN(world_rank) AS best_rank
FROM news_articles
WHERE published_at >= NOW() - INTERVAL '7 days'
GROUP BY source_shortname
ORDER BY best_rank ASC;
```