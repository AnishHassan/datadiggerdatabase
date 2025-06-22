# Table: `glossary_term_tags`

## Description

Join table that maps glossary terms to tags, allowing multiple tags per term and vice versa. Enables flexible classification, filtering, and organization of glossary entries through a tagging system.

---

## Schema

| Column Name | Data Type | Null | Default | Constraints | Description                                       |
| ----------- | --------- | ---- | ------- | ----------- | ------------------------------------------------- |
| term_id     | UUID      | NO   | -       | Foreign Key | References `glossary_terms.id`; the tagged term   |
| tag_id      | UUID      | NO   | -       | Foreign Key | References `tags.id`; the tag applied to the term |

---

## Relationships

| Related Table    | Relationship Type | Foreign Key | Description                                  |
| ---------------- | ----------------- | ----------- | -------------------------------------------- |
| `glossary_terms` | Many-to-One       | `term_id`   | Each entry links to a specific glossary term |
| `tags`           | Many-to-One       | `tag_id`    | Each entry links to a specific tag           |

---

## Indexes

| Index Name                | Column              | Type  | Description                                                |
| ------------------------- | ------------------- | ----- | ---------------------------------------------------------- |
| `glossary_term_tags_pkey` | (term_id, tag_id)   | BTREE | Composite primary key for efficient lookups and uniqueness |

---

## Business Rules

* A unique tag can be applied only once per glossary term.
* Both `term_id` and `tag_id` are required and must reference existing records.

---

## Example Row

```json
{
  "term_id": "dbd97b2c-5a36-465a-8c51-1e8c3fa09fcb",
  "tag_id": "0ff84e78-f723-4d66-9c62-773f2033b3cf"
}
```