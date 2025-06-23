# Table: `related_terms`

---

## Description

The `related_terms` table defines **relationships between glossary terms**. It's a **self-referencing join table** for `glossary_terms`, enabling the modeling of semantic connections like synonyms, related concepts, or associated topics. This enhances user comprehension by enabling contextual navigation.

---

## Schema

| Column Name  | Data Type | Null | Default | Constraints | Description                                                 |
| ------------ | --------- | ---- | ------- | ----------- | ----------------------------------------------------------- |
| `term_id`    | UUID      | NO   | —       | Foreign Key | References `glossary_terms.id` – the main glossary term     |
| `related_id` | UUID      | NO   | —       | Foreign Key | References `glossary_terms.id` – the related or linked term |

---

## Relationships

| Table            | Relationship Type | Foreign Key  | Description                                  |
| ---------------- | ----------------- | ------------ | -------------------------------------------- |
| `glossary_terms` | Many-to-One       | `term_id`    | The base glossary term                       |
| `glossary_terms` | Many-to-One       | `related_id` | The term considered related to the base term |

> Supports **uni-directional** or **bi-directional** linking based on application logic.

---

## Constraints & Indexes

| Index Name           | Columns                   | Type  | Description                                                   |
| -------------------- | ------------------------- | ----- | ------------------------------------------------------------- |
| `related_terms_pkey` | (`term_id`, `related_id`) | BTREE | Enforces uniqueness of each term-to-related-term relationship |

---

## Business Rules

* A `(term_id, related_id)` pair **must be unique** to avoid duplicate relationships.
* Both `term_id` and `related_id` must reference **existing entries** in `glossary_terms`.
* Bi-directional linking (i.e., term A → B and term B → A) must be handled manually unless automated.
* Prevent self-referencing loops (i.e., where `term_id = related_id`) unless explicitly allowed.

---

## Example Record

```json
{
  "term_id": "14bba646-cc2c-4aeb-bc5a-3799e0e8e3d0",
  "related_id": "35a2a3a5-e0e5-476e-b1f2-f0e82a22398b"
}
```

> This represents a link between two glossary terms, e.g., `"API"` is related to `"REST"`.

---

## Query Examples

### Get all related terms for a given term:

```sql
SELECT gt_related.*
FROM related_terms rt
JOIN glossary_terms gt_related ON rt.related_id = gt_related.id
WHERE rt.term_id = '14bba646-cc2c-4aeb-bc5a-3799e0e8e3d0';
```

### Check if two terms are related:

```sql
SELECT 1
FROM related_terms
WHERE (term_id = $1 AND related_id = $2)
   OR (term_id = $2 AND related_id = $1);
```

---