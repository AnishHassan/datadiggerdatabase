# Table: `glossary_terms`

---

## **Description**

Stores glossary entries that define key terms relevant to finance, economics, or the domain’s subject matter. Each record includes a full definition, optional metadata, and a mechanism for categorization and sorting.

---

## **Schema**

| Column Name         | Data Type | Null | Default             | Constraints | Description                                                        |
| ------------------- | --------- | ---- | ------------------- | ----------- | ------------------------------------------------------------------ |
| `id`                | UUID      | NO   | `gen_random_uuid()` | Primary Key | Unique identifier for the glossary term                            |
| `term`              | TEXT      | NO   | –                   |             | The glossary term or keyword                                       |
| `definition`        | TEXT      | NO   | –                   |             | Full detailed definition of the term                               |
| `short_description` | TEXT      | YES  | `NULL`              |             | Optional concise explanation or summary of the term                |
| `level`             | TEXT      | YES  | `NULL`              |             | Describes complexity level (`beginner`, `intermediate`, `expert`)  |
| `first_letter`      | CHAR(1)   | YES  | `NULL`              |             | First letter of the term, used for alphabetical filtering/grouping |
| `updated_at`        | TIMESTAMP | YES  | `CURRENT_TIMESTAMP` |             | Timestamp indicating the last update                               |

---

## **Business Rules**

* `term` and `definition` must be provided for each glossary entry.
* `short_description` is optional but recommended for summary displays.
* `level` can be used to personalize or filter content for users based on proficiency.
* `first_letter` can be precomputed to aid in UI organization (A–Z tabs, etc.).
* `updated_at` can be used for sorting, versioning, or cache invalidation.

---

## **Indexes**

| Index Name            | Column | Type  | Description                                        |
| --------------------- | ------ | ----- | -------------------------------------------------- |
| `glossary_terms_pkey` | `id`   | BTREE | Primary key for unique identification of each term |

---

## **Example Record**

```json
{
  "id": "42fa3ec8-f22d-47ef-87b9-d05bd89c15a3",
  "term": "Quantitative Easing",
  "definition": "A monetary policy whereby a central bank purchases government securities or other securities from the market to increase the money supply and encourage lending and investment.",
  "short_description": "Central bank policy to inject liquidity.",
  "level": "intermediate",
  "first_letter": "Q",
  "updated_at": "2025-06-21T15:00:00"
}
```

---

## **Usage Scenarios**

* Display terms alphabetically grouped by `first_letter`.
* Enable beginner/intermediate/expert-level filtering in learning modules.
* Show `short_description` in search results, tooltips, or cards.

---

## **Query Examples**

### Fetch all terms beginning with "B"

```sql
SELECT term, short_description
FROM glossary_terms
WHERE first_letter = 'B'
ORDER BY term;
```

---

### Retrieve all beginner-level terms

```sql
SELECT term, definition
FROM glossary_terms
WHERE level = 'beginner';
```

---

### Search for terms containing "liquidity"

```sql
SELECT term, definition
FROM glossary_terms
WHERE term ILIKE '%liquidity%' OR definition ILIKE '%liquidity%';
```