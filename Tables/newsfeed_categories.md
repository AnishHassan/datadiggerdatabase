# Table: `newsfeed_categories`

---

## Description

Defines the list of **categories** available for tagging and classifying news feed entries in the `news_feed` table. These categories help improve discoverability, filtering, and user experience by providing contextual labels (e.g., *Economy*, *Technology*, *Geopolitics*).

---

## Schema

| Column Name | Data Type | Null | Default | Constraints | Description                                        |
| ----------- | --------- | ---- | ------- | ----------- | -------------------------------------------------- |
| `id`        | INT       | NO   | –       | Primary Key | Unique identifier for each news category           |
| `name`      | TEXT      | NO   | –       | Unique      | Name of the category (e.g., `'Economy'`, `'Tech'`) |

---

## Relationships

| Related Table | Relationship Type | Foreign Key   | Description                                                      |
| ------------- | ----------------- | ------------- | ---------------------------------------------------------------- |
| `news_feed`   | One-to-Many       | `category_id` | Each news item can belong to a single category via `category_id` |

> *Note: Ensure the `news_feed` table includes a `category_id` column to establish this relationship.*

---

## Business Rules

* `name` must be **unique** and **non-null** for each category.
* Category names should be semantically meaningful and user-facing.
* Categories are intended to support news content organization, filtering, and personalization.

---

## Example Record

```json
{
  "id": 3,
  "name": "Technology"
}
```

---

## Indexes

| Index Name                 | Column | Type  | Description                               |
| -------------------------- | ------ | ----- | ----------------------------------------- |
| `newsfeed_categories_pkey` | id     | BTREE | Ensures unique identification by ID       |
| *(implicit unique index)*  | name   | BTREE | Guarantees that category names are unique |

---

## Usage Scenarios

* **User Filtering**: Allow users to filter news by categories like "Markets", "Tech", "Politics".
* **UI Navigation**: Group and display news articles in tabbed or segmented interfaces.
* **Content Tagging**: Assist in backend content curation and automated categorization.

---

## Query Examples

### List All Categories

```sql
SELECT id, name
FROM newsfeed_categories
ORDER BY name;
```

### Find Category by Name

```sql
SELECT *
FROM newsfeed_categories
WHERE name ILIKE 'Tech%';
```