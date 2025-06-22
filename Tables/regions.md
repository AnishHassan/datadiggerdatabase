# Table: `regions`

## Description

Stores global or national regions that may be associated with users, datasets, or industry data. It optionally includes a visual representation via a flag URL.

---

## Schema

| Column Name | Data Type | Null | Default | Constraints | Description                                     |
| ----------- | --------- | ---- | ------- | ----------- | ----------------------------------------------- |
| id          | INT       | NO   | -       | Primary Key | Unique identifier for each region               |
| name        | TEXT      | NO   | -       | Unique      | Name of the region (e.g., country or area name) |
| flag_url    | TEXT      | YES  | NULL    |             | URL to a flag image representing the region     |

---

## Indexes

| Index Name         | Column | Type  | Description                             |
| ------------------ | ------ | ----- | --------------------------------------- |
| `regions_pkey`     | id     | BTREE | Primary key for region ID               |
| `regions_name_key` | name   | BTREE | Unique constraint to prevent duplicates |

---

## Example Row

```json
{
  "id": 1,
  "name": "Germany",
  "flag_url": "https://example.com/flags/germany.png"
}
```