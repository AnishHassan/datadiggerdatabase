# Table: `news_sources`

---

## Description

Stores metadata for official news publishers. This includes the name and optional logo of each source, enabling clear attribution and branding across all content originating from external news providers.

---

## Schema

| Column Name | Data Type | Null | Default | Constraints | Description                                     |
| ----------- | --------- | ---- | ------- | ----------- | ----------------------------------------------- |
| `id`        | INT       | NO   | -       | Primary Key | Unique identifier for the news source           |
| `name`      | TEXT      | NO   | -       | Unique      | Display name of the news source (e.g., Reuters) |
| `logo_url`  | TEXT      | YES  | NULL    |             | Optional URL pointing to the news source’s logo |

---

## Relationships

| Referencing Table | Foreign Key   | Relationship | Description                                           |
| ----------------- | ------------- | ------------ | ----------------------------------------------------- |
| `news_feed`       | `source_id`   | One-to-Many  | Connects each news feed item to its attributed source |
| `news_wire`       | `source_id`\* | One-to-Many  | May also link structured wire entries to their source |

> *Note: Only `news_feed` enforces a defined foreign key; other tables like `news_wire` may reference this logically.*

---

## Example Record

```json
{
  "id": 1,
  "name": "Bloomberg",
  "logo_url": "https://example.com/logos/bloomberg.png"
}
```

---

## Usage Scenarios

* **Attribution**: Display the source name and logo alongside headlines and news content.
* **Filtering**: Filter or sort news by specific sources (e.g., only show Reuters news).
* **Normalization**: Avoid storing duplicate source names in each news table by referencing this table via `source_id`.

---

## Indexes

| Index Name              | Column | Type  | Purpose                                           |
| ----------------------- | ------ | ----- | ------------------------------------------------- |
| `news_sources_pkey`     | `id`   | BTREE | Ensures uniqueness of each news source            |
| `news_sources_name_key` | `name` | BTREE | Enforces unique source names and optimizes lookup |

---

## Query Examples

### Get All News Sources

```sql
SELECT * FROM news_sources
ORDER BY name ASC;
```

### Find News Source by Name

```sql
SELECT id, name, logo_url
FROM news_sources
WHERE name ILIKE '%bloomberg%';
```

### Join with News Feed for Attribution

```sql
SELECT nf.headline, ns.name AS source_name, ns.logo_url
FROM news_feed nf
JOIN news_sources ns ON nf.source_id = ns.id
WHERE nf.publish_date = CURRENT_DATE;
```

---

## Insert Example

```sql
INSERT INTO news_sources (id, name, logo_url)
VALUES (
  1,
  'Bloomberg',
  'https://example.com/logos/bloomberg.png'
);
```