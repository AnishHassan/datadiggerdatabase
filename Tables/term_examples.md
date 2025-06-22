# Table: `term_examples`

## Description

Holds example sentences or usage scenarios for glossary terms to illustrate their meaning in context. Enhances user understanding through practical demonstrations of term usage.

---

## Schema

| Column Name | Data Type | Null | Default             | Constraints | Description                                                |
| ----------- | --------- | ---- | ------------------- | ----------- | ---------------------------------------------------------- |
| id          | UUID      | NO   | `gen_random_uuid()` | Primary Key | Unique identifier for each example                         |
| term_id     | UUID      | YES  | -                   | Foreign Key | References the `glossary_terms.id` this example relates to |
| example     | TEXT      | NO   | -                   |             | Example usage or explanation text for the term             |

---

## Relationships

| Related Table    | Relationship Type | Foreign Key | Description                                 |
| ---------------- | ----------------- | ----------- | ------------------------------------------- |
| `glossary_terms` | Many-to-One       | `term_id`   | Links the example to the corresponding term |

---

## Business Rules

* Each example must contain valid, clear text illustrating a glossary term.
* `term_id` is optional to allow standalone examples (though typically associated).
* `example` must not be empty.

---

## Indexes

| Index Name           | Column | Type  | Description                           |
| -------------------- | ------ | ----- | ------------------------------------- |
| `term_examples_pkey` | id     | BTREE | Primary key for unique identification |

---

## Example Row

```json
{
  "id": "b7de23a1-99a5-4e09-8b43-1de5e4528d29",
  "term_id": "e4c3c5d2-5e9a-402e-a13a-0ec1c5e45f02",
  "example": "Inflation occurs when the general price level of goods and services rises over time."
}
```