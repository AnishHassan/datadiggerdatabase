# Table: `rss_feed_categories`

### **Purpose**

A **junction table** (many-to-many relationship) that links RSS feeds to one or more categories. Each row indicates that a given RSS feed (`feed_id`) is assigned to a specific category (`category_id`).

---

## Schema

| Column Name   | Data Type | Null | Constraints                            | Description                   |
| ------------- | --------- | ---- | -------------------------------------- | ----------------------------- |
| `feed_id`     | INT4      | NO   | Foreign Key → `rss_feeds.id`           | ID of the RSS feed            |
| `category_id` | INT4      | NO   | Foreign Key → `calendar_categories.id` | ID of the associated category |

---

## Indexes

| Index Name                            | Columns                    | Description                               |
| ------------------------------------- | -------------------------- | ----------------------------------------- |
| `rss_feed_categories_pkey`            | (`feed_id`, `category_id`) | Composite unique key, prevents duplicates |
| `idx_rss_feed_categories_category_id` | `category_id`              | Optimizes queries filtering by category   |

---

## Relationships

* `feed_id` → `rss_feeds.id`
* `category_id` → `calendar_categories.id`

---

## Example Record

```json
{
  "feed_id": 17,
  "category_id": 3
}
```

This means RSS Feed #17 is assigned to Category #3.

---

## Usage Scenarios

* A feed can belong to multiple categories (e.g., “Politics” and “World”).
* A category can contain articles from multiple feeds.

---

## Query Examples

### Get all categories assigned to a specific feed

```sql
SELECT c.id, c.name
FROM rss_feed_categories rfc
JOIN calendar_categories c ON rfc.category_id = c.id
WHERE rfc.feed_id = 17;
```

### Get all feeds under a specific category

```sql
SELECT f.id, f.feed_name
FROM rss_feed_categories rfc
JOIN rss_feeds f ON rfc.feed_id = f.id
WHERE rfc.category_id = 3;
```

### Find all feed-category relationships

```sql
SELECT feed_id, category_id
FROM rss_feed_categories;
```