# Table: `tags`

## Description

Stores unique tag values that can be used to label or categorize various entities (e.g., news articles, events, glossary terms) across the platform. Tags facilitate flexible filtering, grouping, and discoverability.

---

## Schema

| Column Name | Data Type | Null | Default             | Constraints | Description                               |
| ----------- | --------- | ---- | ------------------- | ----------- | ----------------------------------------- |
| id          | UUID      | NO   | `gen_random_uuid()` | Primary Key | Unique identifier for each tag            |
| name        | TEXT      | NO   | -                   | Unique      | The label or keyword representing the tag |

---

## Business Rules

* `name` must be unique and is required.
* Tags can be used across multiple tables/entities via mapping tables (e.g., `entity_tags`, `news_tags`).

---

## Indexes

| Index Name      | Column | Type  | Description                               |
| --------------- | ------ | ----- | ----------------------------------------- |
| `tags_pkey`     | id     | BTREE | Primary key for unique tag identification |
| `tags_name_key` | name   | BTREE | Ensures tag names remain unique           |

---

## Example Row

```json
{
  "id": "dc8b7e5b-63a7-489b-8d77-32ef8a1c26d6",
  "name": "inflation"
}
```