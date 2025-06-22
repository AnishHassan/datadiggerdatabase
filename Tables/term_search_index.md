# Table: `term_search_index`

## Description

Supports full-text search capabilities for glossary terms by maintaining a searchable representation of term content. It enables fast and efficient text-based querying using PostgreSQL's `tsvector` and GIN indexing.

---

## Schema

| Column Name | Data Type | Null | Default | Constraints | Description                                           |
| ----------- | --------- | ---- | ------- | ----------- | ----------------------------------------------------- |
| term_id     | UUID      | NO   | -       | Primary Key | References the `glossary_terms.id`; unique identifier |
| content     | TEXT      | YES  | -       |             | Plain text content to be indexed                      |
| tsv         | TSVECTOR  | YES  | -       |             | Preprocessed tsvector used for full-text indexing     |

---

## Relationships

| Related Table    | Relationship Type | Foreign Key | Description                                            |
| ---------------- | ----------------- | ----------- | ------------------------------------------------------ |
| `glossary_terms` | One-to-One        | `term_id`   | Direct mapping of a term to its searchable index entry |

---

## Business Rules

* Each `term_id` must be unique and correspond to one glossary term.
* `tsv` is generated (typically via trigger or function) from the `content` field.
* Indexing is optimized for full-text search queries.

---

## Indexes

| Index Name            | Column | Type | Description                                       |
| --------------------- | ------ | ---- | ------------------------------------------------- |
| `term_search_tsv_idx` | tsv    | GIN  | Supports fast full-text search over glossary data |

---

## Example Row

```json
{
  "term_id": "b8fc423d-97ab-4b16-afe7-2f9a39eb785c",
  "content": "Gross Domestic Product (GDP) is a monetary measure of the market value of all the final goods and services produced in a specific time period.",
  "tsv": "'domestic':2 'final':11 'gdp':5 'goods':10 'gross':1 'market':7 'measure':4 'monetari':3 'period':17 'product':6 'produc':14 'servic':12 'specif':15 'time':16 'valu':8"
}
```