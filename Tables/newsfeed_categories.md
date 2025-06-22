# Table: `newsfeed_categories`

## Description

Defines a set of categories for classifying entries in the `news_feed` table. Helps in organizing, filtering, and tagging news content for better user navigation and contextual relevance.

---

## Schema

| Column Name | Data Type | Null | Default | Constraints | Description                                |
| ----------- | --------- | ---- | ------- | ----------- | ------------------------------------------ |
| id          | INT       | NO   | -       | Primary Key | Unique identifier for the category         |
| name        | TEXT      | NO   | -       | Unique      | Name of the category (e.g., Economy, Tech) |

---

## Relationships

| Related Table | Relationship Type | Foreign Key  | Description                                    |
| ------------- | ----------------- | ------------ | ---------------------------------------------- |
| `news_feed`   | One-to-Many       | category_id  | Each news feed item may belong to one category |

> *(Note: `category_id` column would need to exist in the `news_feed` table to establish this relationship.)*

---

## Business Rules

* Each category name must be unique.
* Used for tagging and filtering news items by theme or subject matter.

---

## Indexes

| Index Name                 | Column | Type  | Description                             |
| -------------------------- | ------ | ----- | --------------------------------------- |
| `newsfeed_categories_pkey` | id     | BTREE | Primary key for unique identification   |
| *(implicit unique index)*  | name   | BTREE | Ensures category names are not repeated |

---

## Example Row

```json
{
  "id": 3,
  "name": "Technology"
}
```