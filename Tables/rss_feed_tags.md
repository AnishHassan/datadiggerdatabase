# Table: `rss_feed_tags`

---

## Purpose

A **junction table** enabling a **many-to-many relationship** between RSS feeds and tags. It allows each feed to be associated with multiple tags, and each tag to be used across many feeds.

---

## Schema

| Column Name | Data Type | Null | Constraints                       | Description                                |
| ----------- | --------- | ---- | --------------------------------- | ------------------------------------------ |
| `feed_id`   | INT4      | NO   | Foreign Key → `rss_feeds.id`      | ID of the RSS feed                         |
| `tag_id`    | UUID      | NO   | Foreign Key → `tags.id` (assumed) | UUID of the tag (referencing `tags` table) |

> **Composite primary key** on (`feed_id`, `tag_id`) ensures each pairing is unique.

---

## Indexes

| Index Name                 | Columns               | Purpose                                    |
| -------------------------- | --------------------- | ------------------------------------------ |
| `rss_feed_tags_pkey`       | (`feed_id`, `tag_id`) | Prevents duplicate tag assignments         |
| `idx_rss_feed_tags_tag_id` | `tag_id`              | Speeds up tag-based filtering and searches |

---

## Relationships

| Column    | References       | Relationship |
| --------- | ---------------- | ------------ |
| `feed_id` | `rss_feeds.id`   | Many-to-One  |
| `tag_id`  | `tags.id` (UUID) | Many-to-One  |

> Together, these relationships support a **many-to-many** linkage between `rss_feeds` and `tags`.

---

## Example Record

```json
{
  "feed_id": 5,
  "tag_id": "c2f81b4e-fc61-4b2c-a8a9-e04d8e32bcd9"
}
```

This maps RSS Feed #5 to the tag with UUID `c2f81b4e-fc61-4b2c-a8a9-e04d8e32bcd9`.

---

## Use Cases

* Apply dynamic or user-defined tagging to feeds.
* Power filtering by topic (e.g., “crypto”, “health”, “breaking-news”).
* Enable tag-based personalization or discovery.

---

## Example Queries

### Tags for a specific feed

```sql
SELECT t.*
FROM rss_feed_tags rft
JOIN tags t ON rft.tag_id = t.id
WHERE rft.feed_id = 5;
```

### Feeds for a specific tag

```sql
SELECT f.*
FROM rss_feed_tags rft
JOIN rss_feeds f ON rft.feed_id = f.id
WHERE rft.tag_id = 'c2f81b4e-fc61-4b2c-a8a9-e04d8e32bcd9';
```

### Feed count per tag

```sql
SELECT tag_id, COUNT(*) AS feed_count
FROM rss_feed_tags
GROUP BY tag_id;
```

---