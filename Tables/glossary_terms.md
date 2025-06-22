# Table: `glossary_terms`

## Description

Stores glossary entries that define important terms relevant to finance, economics, or the domain's subject matter. Includes optional metadata for grouping, filtering, and display purposes.

---

## Schema

| Column Name        | Data Type | Null | Default             | Constraints | Description                                                       |
| ------------------ | --------- | ---- | ------------------- | ----------- | ----------------------------------------------------------------- |
| id                 | UUID      | NO   | `gen_random_uuid()` | Primary Key | Unique identifier for the glossary term                           |
| term               | TEXT      | NO   | -                   |             | The glossary term or keyword                                      |
| definition         | TEXT      | NO   | -                   |             | Full detailed definition of the term                              |
| short_description  | TEXT      | YES  | NULL                |             | Optional concise explanation or summary                           |
| level              | TEXT      | YES  | NULL                |             | Describes complexity level (e.g., beginner, intermediate, expert) |
| first_letter       | CHAR(1)   | YES  | NULL                |             | First character of the term, used for alphabetical grouping       |
| updated_at         | TIMESTAMP | YES  | `CURRENT_TIMESTAMP` |             | Timestamp indicating last update                                  |

---

## Business Rules

* `term` and `definition` are mandatory for every glossary entry.
* `first_letter` is optional but useful for alphabetical categorization.
* `level` can be used to filter terms by user proficiency.

---

## Indexes

| Index Name            | Column | Type  | Description                                        |
| --------------------- | ------ | ----- | -------------------------------------------------- |
| `glossary_terms_pkey` | id     | BTREE | Primary key for unique identification of each term |

---

## Example Row

```json
{
  "id": "42fa3ec8-f22d-47ef-87b9-d05bd89c15a3",
  "term": "Quantitative Easing",
  "definition": "A monetary policy whereby a central bank purchases government securities or other securities from the market...",
  "short_description": "Central bank policy to inject liquidity.",
  "level": "intermediate",
  "first_letter": "Q",
  "updated_at": "2025-06-21T15:00:00"
}
```