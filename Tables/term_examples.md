# Table: `term_examples`

### **Description**

Stores contextual usage examples for glossary terms, helping users understand each term through clear, practical sentences or scenarios. These examples enhance comprehension and educational value.

---

## **Schema**

| Column Name | Data Type | Null | Constraints                               | Description                                             |
| ----------- | --------- | ---- | ----------------------------------------- | ------------------------------------------------------- |
| `id`        | UUID      | NO   | Primary Key, Default: `gen_random_uuid()` | Unique identifier for each example record               |
| `term_id`   | UUID      | YES  | Foreign Key → `glossary_terms.id`         | Reference to the glossary term this example belongs to  |
| `example`   | TEXT      | NO   |                                           | Text providing a clear usage or explanation of the term |

---

## **Relationships**

| Related Table    | Relationship Type | Foreign Key | Description                                    |
| ---------------- | ----------------- | ----------- | ---------------------------------------------- |
| `glossary_terms` | Many-to-One       | `term_id`   | Each example links to a specific glossary term |

> Allows flexibility by permitting `term_id` to be null for generic or unassigned examples.

---

## **Business Rules**

* `example` must be meaningful and non-empty.
* `term_id` is optional to support draft or global examples not yet assigned.
* Typically, each glossary term will have one or more related examples.

---

## **Indexes**

| Index Name                             | Column    | Type  | Description                                  |
| -------------------------------------- | --------- | ----- | -------------------------------------------- |
| `term_examples_pkey`                   | `id`      | BTREE | Primary key for fast lookup by ID            |
| (optional) `idx_term_examples_term_id` | `term_id` | BTREE | Improves join/query performance on `term_id` |

---

## **Example Record**

```json
{
  "id": "b7de23a1-99a5-4e09-8b43-1de5e4528d29",
  "term_id": "e4c3c5d2-5e9a-402e-a13a-0ec1c5e45f02",
  "example": "Inflation occurs when the general price level of goods and services rises over time."
}
```

---

## **Usage Scenarios**

* Show real-world examples of glossary terms in educational UI or tooltips.
* Auto-display examples in search, glossary, or content detail views.
* Support onboarding or learning features through illustrative examples.

---

## **Query Examples**

### Get all examples for a given term:

```sql
SELECT example
FROM term_examples
WHERE term_id = 'e4c3c5d2-5e9a-402e-a13a-0ec1c5e45f02';
```

### List all unassigned examples:

```sql
SELECT *
FROM term_examples
WHERE term_id IS NULL;
```

### Search examples containing a keyword:

```sql
SELECT *
FROM term_examples
WHERE example ILIKE '%price level%';
```

---

## **Insert Example**

```sql
INSERT INTO term_examples (term_id, example)
VALUES (
  'e4c3c5d2-5e9a-402e-a13a-0ec1c5e45f02',
  'Inflation occurs when the general price level of goods and services rises over time.'
);
```

---