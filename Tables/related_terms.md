# Table: `related_terms`

## Description

A self-referencing join table for glossary terms that defines relationships between terms. Useful for displaying related concepts, synonyms, or linked definitions to enhance contextual understanding.

---

## Schema

| Column Name | Data Type | Null | Default | Constraints | Description                                               |
| ----------- | --------- | ---- | ------- | ----------- | --------------------------------------------------------- |
| term_id     | UUID      | NO   | -       | Foreign Key | References `glossary_terms.id`; the main glossary term    |
| related_id  | UUID      | NO   | -       | Foreign Key | References `glossary_terms.id`; the related glossary term |

---

## Relationships

| Related Table    | Relationship Type | Foreign Key  | Description                               |
| ---------------- | ----------------- | ------------ | ----------------------------------------- |
| `glossary_terms` | Many-to-One       | `term_id`    | Links to the primary glossary term        |
| `glossary_terms` | Many-to-One       | `related_id` | Links to the term considered as "related" |

---

## Indexes

| Index Name           | Column                  | Type  | Description                                      |
| -------------------- | ----------------------- | ----- | ------------------------------------------------ |
| `related_terms_pkey` | (term_id, related_id)   | BTREE | Composite key ensuring unique related term pairs |

---

## Business Rules

* The pair `(term_id, related_id)` must be unique to avoid redundant relationships.
* Both `term_id` and `related_id` must reference existing `glossary_terms`.
* This table enables bi-directional or uni-directional term relationships.

---

## Example Row

```json
{
  "term_id": "14bba646-cc2c-4aeb-bc5a-3799e0e8e3d0",
  "related_id": "35a2a3a5-e0e5-476e-b1f2-f0e82a22398b"
}
```