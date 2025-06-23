# Table: `newsfeed_category_map`

---

## Description

Join table enabling a **many-to-many relationship** between `news_feed` items and `newsfeed_categories`.
This structure allows a single news item to be associated with **multiple categories**, and each category to be linked to **multiple news entries**.

---

## Schema

| Column Name   | Data Type | Null | Default | Constraints | Description                                              |
| ------------- | --------- | ---- | ------- | ----------- | -------------------------------------------------------- |
| `newsfeed_id` | UUID      | NO   | –       | Foreign Key | References a record in the `news_feed` table (`id`)      |
| `category_id` | INT       | NO   | –       | Foreign Key | References a category in the `newsfeed_categories` table |

---

## Relationships

| Related Table         | Relationship Type | Foreign Key   | Description                                              |
| --------------------- | ----------------- | ------------- | -------------------------------------------------------- |
| `news_feed`           | Many-to-One       | `newsfeed_id` | Each mapping ties to a news feed entry                   |
| `newsfeed_categories` | Many-to-One       | `category_id` | Each mapping ties to a valid category for that news item |

> Together, the `newsfeed_id` and `category_id` form a **composite primary key** that ensures uniqueness per pair.

---

## Business Rules

* Composite primary key: (`newsfeed_id`, `category_id`)
* Prevents duplicate mappings between the same news feed and category.
* Maintains normalized data for efficient tagging and querying.

---

## Indexes

| Index Name                   | Column(s)                      | Type  | Description                                    |
| ---------------------------- | ------------------------------ | ----- | ---------------------------------------------- |
| `newsfeed_category_map_pkey` | (`newsfeed_id`, `category_id`) | BTREE | Composite primary key; ensures unique mappings |

---

## Example Record

```json
{
  "newsfeed_id": "cb6d13b2-e120-4e32-b0e1-a5dd2cf4a0de",
  "category_id": 5
}
```

---

## Usage Scenarios

* Assign multiple relevant categories (e.g., *Politics*, *Europe*) to a single news article.
* Retrieve all articles under a specific category for filtering or display.
* Support advanced queries like multi-category recommendations or tag-based search.

---

## Query Examples

### Get All Categories for a Given Newsfeed Item

```sql
SELECT c.name
FROM newsfeed_category_map m
JOIN newsfeed_categories c ON m.category_id = c.id
WHERE m.newsfeed_id = 'cb6d13b2-e120-4e32-b0e1-a5dd2cf4a0de';
```

### Find All News Items Tagged with a Specific Category

```sql
SELECT nf.*
FROM newsfeed_category_map m
JOIN news_feed nf ON m.newsfeed_id = nf.id
WHERE m.category_id = 5;
```