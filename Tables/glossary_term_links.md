# Table: `glossary_term_links`

## Description

Stores external or internal reference links associated with glossary terms. These links provide users with additional resources for deeper understanding, such as articles, official definitions, or learning materials.

---

## Schema

| Column Name | Data Type | Null | Default             | Constraints | Description                                               |
| ----------- | --------- | ---- | ------------------- | ----------- | --------------------------------------------------------- |
| id          | UUID      | NO   | `gen_random_uuid()` | Primary Key | Unique identifier for each link                           |
| term_id     | UUID      | YES  | -                   | Foreign Key | References the `glossary_terms.id` the link is related to |
| title       | TEXT      | NO   | -                   |             | Title or short description of the link                    |
| url         | TEXT      | NO   | -                   |             | The actual hyperlink destination                          |

---

## Relationships

| Related Table    | Relationship Type | Foreign Key | Description                                       |
| ---------------- | ----------------- | ----------- | ------------------------------------------------- |
| `glossary_terms` | Many-to-One       | `term_id`   | Associates the link with a specific glossary term |

---

## Business Rules

* Each link must have a non-empty `title` and `url`.
* `term_id` is optional to allow general educational links (if needed).
* URLs must be well-formed and preferably HTTPS.

---

## Indexes

| Index Name                 | Column | Type  | Description                           |
| -------------------------- | ------ | ----- | ------------------------------------- |
| `glossary_term_links_pkey` | id     | BTREE | Primary key for unique identification |

---

## Example Row

```json
{
  "id": "d23fa9c3-f1b2-4b33-9d5f-4a678ae11b5a",
  "term_id": "8bcf108d-cf5e-42e2-9e89-01353e94fc60",
  "title": "Investopedia: GDP Definition",
  "url": "https://www.investopedia.com/terms/g/gdp.asp"
}
```