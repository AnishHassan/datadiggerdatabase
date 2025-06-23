# Table: `rss_feed_categories`

---

## Purpose

`rss_feed_categories` is a **junction table** establishing a **many-to-many relationship** between RSS feeds and categories. It enables assigning multiple categories to a feed and vice versa.

---

## Schema

| Column Name   | Data Type | Null | Constraints                            | Description                            |
| ------------- | --------- | ---- | -------------------------------------- | -------------------------------------- |
| `feed_id`     | INT4      | NO   | Foreign Key → `rss_feeds.id`           | ID of the RSS feed                     |
| `category_id` | INT4      | NO   | Foreign Key → `calendar_categories.id` | ID of the associated calendar category |

> 💡 Both fields together form the **composite primary key**.

---

## Indexes

| Index Name                            | Columns                    | Purpose                                     |
| ------------------------------------- | -------------------------- | ------------------------------------------- |
| `rss_feed_categories_pkey`            | (`feed_id`, `category_id`) | Ensures each pair is unique (no duplicates) |
| `idx_rss_feed_categories_category_id` | `category_id`              | Optimizes lookups by category               |

---

## Relationships

| Column        | References               | Cardinality |
| ------------- | ------------------------ | ----------- |
| `feed_id`     | `rss_feeds.id`           | Many-to-One |
| `category_id` | `calendar_categories.id` | Many-to-One |

> Together, these support a **many-to-many** relationship between `rss_feeds` and `calendar_categories`.

---

## Use Cases

* Group feeds under content themes (e.g., “Politics”, “Tech”).
* Aggregate feeds for specific content filtering.
* Support category-based subscription filters or discovery UX.

---

## Example Record

```json
{
  "feed_id": 17,
  "category_id": 3
}
```

This means *RSS Feed #17* is associated with *Category #3*.

---

## Query Examples

### Get all categories for a given feed

```sql
SELECT c.id, c.name
FROM rss_feed_categories rfc
JOIN calendar_categories c ON rfc.category_id = c.id
WHERE rfc.feed_id = 17;
```

### Get all feeds for a given category

```sql
SELECT f.id, f.feed_name
FROM rss_feed_categories rfc
JOIN rss_feeds f ON rfc.feed_id = f.id
WHERE rfc.category_id = 3;
```

### Get all feed-category mappings

```sql
SELECT feed_id, category_id
FROM rss_feed_categories;
```

---