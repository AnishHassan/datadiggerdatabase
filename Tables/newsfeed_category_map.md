# Table: `newsfeed_category_map`

## Description

Join table to support a many-to-many relationship between `news_feed` entries and `newsfeed_categories`. Enables a single news item to be tagged with multiple categories, and a category to be applied to multiple news items.

---

## Schema

| Column Name  | Data Type | Null | Default | Constraints | Description                                              |
| ------------ | --------- | ---- | ------- | ----------- | -------------------------------------------------------- |
| newsfeed_id  | UUID      | NO   | -       | Foreign Key | References a news entry in the `news_feed` table         |
| category_id  | INT       | NO   | -       | Foreign Key | References a category in the `newsfeed_categories` table |

---

## Relationships

| Related Table         | Relationship Type | Foreign Key  | Description                                             |
| --------------------- | ----------------- | ------------ | ------------------------------------------------------- |
| `news_feed`           | Many-to-One       | newsfeed_id  | Connects to a news item                                 |
| `newsfeed_categories` | Many-to-One       | category_id  | Connects to a category associated with the news content |

---

## Business Rules

* Composite key (`newsfeed_id`, `category_id`) ensures no duplicate mappings.
* Ensures flexible, normalized design for multi-category tagging.

---

## Indexes

| Index Name                   | Column(s)                    | Type  | Description                                             |
| ---------------------------- | ---------------------------- | ----- | ------------------------------------------------------- |
| `newsfeed_category_map_pkey` | (newsfeed_id, category_id)   | BTREE | Composite index to optimize join and prevent duplicates |

---

## Example Row

```json
{
  "newsfeed_id": "cb6d13b2-e120-4e32-b0e1-a5dd2cf4a0de",
  "category_id": 5
}
```