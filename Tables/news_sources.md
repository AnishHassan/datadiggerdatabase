# Table: `news_sources`

## Description

Stores metadata for various news providers. This includes the name and logo of the source, allowing for attribution and branding across news-related features within the system.

---

## Schema

| Column Name | Data Type | Null | Default | Constraints | Description                                   |
| ----------- | --------- | ---- | ------- | ----------- | --------------------------------------------- |
| id          | INT       | NO   | -       | Primary Key | Unique identifier for each news source        |
| name        | TEXT      | NO   | -       | Unique      | Name of the news source (e.g., Reuters, CNBC) |
| logo_url    | TEXT      | YES  | NULL    |             | Optional URL to the logo of the news source   |

---

## Relationships

This table is typically referenced by other news-related tables (e.g., `news_wire`) for structured attribution, though no explicit foreign key is defined in the schema above.

---

## Business Rules

* `name` must be unique and cannot be null.
* `logo_url` is optional and used for display or branding purposes.
* This table supports normalization by removing repeated source info in news entries.

---

## Indexes

| Index Name              | Column | Type  | Description                                      |
| ----------------------- | ------ | ----- | ------------------------------------------------ |
| `news_sources_pkey`     | id     | BTREE | Primary key for fast access                      |
| `news_sources_name_key` | name   | BTREE | Enforces uniqueness and optimizes lookup by name |

---

## Example Row

```json
{
  "id": 1,
  "name": "Bloomberg",
  "logo_url": "https://example.com/logos/bloomberg.png"
}
```