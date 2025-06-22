# Table: `rss_feed_tags`

### **Purpose**

A **junction table** that defines a **many-to-many relationship** between RSS feeds and tags. Each row indicates that a specific tag is associated with a given RSS feed.

---

## Schema

| Column Name | Data Type | Null | Description                            |
| ----------- | --------- | ---- | -------------------------------------- |
| `feed_id`   | INT4      | NO   | Foreign Key → `rss_feeds.id`           |
| `tag_id`    | UUID      | NO   | Foreign Key → a `tags` table (assumed) |

> The `tag_id` references a UUID, suggesting a separate `tags` table likely exists, where each tag has a unique identifier.

---

## Indexes

| Index Name                 | Columns               | Description                                |
| -------------------------- | --------------------- | ------------------------------------------ |
| `rss_feed_tags_pkey`       | (`feed_id`, `tag_id`) | Composite primary key to ensure uniqueness |
| `idx_rss_feed_tags_tag_id` | `tag_id`              | Optimizes filtering queries by tag         |

---

## Relationships

* `feed_id` → `rss_feeds.id`
* `tag_id` → Presumed foreign key to a `tags` table (not shown yet)

---

## Example Record

```json
{
  "feed_id": 5,
  "tag_id": "c2f81b4e-fc61-4b2c-a8a9-e04d8e32bcd9"
}
```

This means RSS Feed #5 is tagged with the tag having UUID `c2f81b4e-fc61-4b2c-a8a9-e04d8e32bcd9`.

---

## Use Cases

* Tagging feeds with topics, e.g., “Technology”, “Finance”, “Breaking News”.
* Filtering or categorizing feeds by specific user-defined or system-defined tags.

---

## Example Queries

### Get all tags for a specific feed

```sql
SELECT t.*
FROM rss_feed_tags rft
JOIN tags t ON rft.tag_id = t.id
WHERE rft.feed_id = 5;
```

### Get all feeds that have a specific tag

```sql
SELECT f.*
FROM rss_feed_tags rft
JOIN rss_feeds f ON rft.feed_id = f.id
WHERE rft.tag_id = 'c2f81b4e-fc61-4b2c-a8a9-e04d8e32bcd9';
```

### Count how many feeds are tagged with each tag

```sql
SELECT tag_id, COUNT(*) AS feed_count
FROM rss_feed_tags
GROUP BY tag_id;
```