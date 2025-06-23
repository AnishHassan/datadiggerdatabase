# Table: `regions`

---

## Description

The `regions` table stores global or national regions (e.g., countries, territories) which can be associated with users, datasets, or industry-specific data. Each region has a **unique name** and may optionally include a **visual identifier**, such as a flag image URL.

---

## Schema

| Column Name | Data Type | Null | Default | Constraints | Description                                         |
| ----------- | --------- | ---- | ------- | ----------- | --------------------------------------------------- |
| `id`        | INT       | NO   | —       | Primary Key | Unique internal identifier for each region          |
| `name`      | TEXT      | NO   | —       | Unique      | Human-readable name of the region (e.g., "Germany") |
| `flag_url`  | TEXT      | YES  | NULL    | —           | Optional URL to an image representing the region    |

---

## Relationships

This table is commonly referenced in other tables to provide **regional context**. Typical associations include:

* Users (e.g., user profile region)
* Datasets (e.g., region of data collection)
* Reports or dashboards with geographic filters

> Add explicit foreign key constraints in related tables for referential integrity.

---

## Indexes

| Index Name         | Column | Type  | Description                         |
| ------------------ | ------ | ----- | ----------------------------------- |
| `regions_pkey`     | `id`   | BTREE | Primary key index for region ID     |
| `regions_name_key` | `name` | BTREE | Enforces uniqueness of region names |

---

## Business Rules

* `name` must be unique and descriptive (e.g., avoid codes like "DE"; prefer "Germany").
* `flag_url` is optional and should point to a valid static image (e.g., SVG or PNG).
* Regional names should follow a consistent format (e.g., title case) and use localized variants when appropriate.

---

## Example Record

```json
{
  "id": 1,
  "name": "Germany",
  "flag_url": "https://example.com/flags/germany.png"
}
```

---

## Query Examples

### Retrieve all regions with flags:

```sql
SELECT * FROM regions
WHERE flag_url IS NOT NULL;
```

### Find a region by name (case-insensitive):

```sql
SELECT * FROM regions
WHERE LOWER(name) = 'germany';
```