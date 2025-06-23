# Table: `term_search_index`

### **Description**

Provides full-text search support for glossary terms by storing a `tsvector`-processed version of each term’s descriptive content. This enables performant keyword-based lookup using PostgreSQL’s full-text search capabilities (e.g., via `@@` queries and GIN indexes).

---

## **Schema**

| Column Name | Data Type | Null | Default | Constraints                                    | Description                                                  |
| ----------- | --------- | ---- | ------- | ---------------------------------------------- | ------------------------------------------------------------ |
| `term_id`   | UUID      | NO   | -       | Primary Key, Foreign Key → `glossary_terms.id` | Unique ID referencing a glossary term                        |
| `content`   | TEXT      | YES  | -       |                                                | Raw text content describing the glossary term                |
| `tsv`       | TSVECTOR  | YES  | -       |                                                | Parsed vectorized content used for full-text indexing/search |

---

## **Relationships**

| Related Table    | Relationship Type | Foreign Key | Description                                                 |
| ---------------- | ----------------- | ----------- | ----------------------------------------------------------- |
| `glossary_terms` | One-to-One        | `term_id`   | Associates a glossary term with its corresponding FTS index |

---

## **Business Rules**

* `term_id` must be unique and directly map to an existing glossary term.
* `tsv` is typically computed automatically from `content` via triggers or update functions (e.g., `to_tsvector('english', content)`).
* Supports keyword search via `tsv @@ to_tsquery('keyword')` or `plainto_tsquery`.

---

## **Indexes**

| Index Name            | Column | Type | Description                                               |
| --------------------- | ------ | ---- | --------------------------------------------------------- |
| `term_search_tsv_idx` | `tsv`  | GIN  | GIN index for efficient full-text search over `tsv` field |

> **Note:** Consider reindexing after bulk updates to keep the search index optimized.

---

## **Example Record**

```json
{
  "term_id": "b8fc423d-97ab-4b16-afe7-2f9a39eb785c",
  "content": "Gross Domestic Product (GDP) is a monetary measure of the market value of all the final goods and services produced in a specific time period.",
  "tsv": "'domestic':2 'final':11 'gdp':5 'goods':10 'gross':1 'market':7 'measure':4 'monetari':3 'period':17 'product':6 'produc':14 'servic':12 'specif':15 'time':16 'valu':8"
}
```

---

## **Usage Scenarios**

* Enables search autocomplete or suggestion features for glossary terms.
* Facilitates fuzzy matching of terms through full-text search functions.
* Supports semantic or linguistic query parsing with stemming and weighting.

---

## **Query Examples**

### Full-text search for keyword "GDP":

```sql
SELECT gt.title, tsi.content
FROM term_search_index tsi
JOIN glossary_terms gt ON gt.id = tsi.term_id
WHERE tsi.tsv @@ plainto_tsquery('GDP');
```

### Rank results by relevance:

```sql
SELECT gt.title, tsi.content, ts_rank(tsi.tsv, plainto_tsquery('monetary value')) AS rank
FROM term_search_index tsi
JOIN glossary_terms gt ON gt.id = tsi.term_id
WHERE tsi.tsv @@ plainto_tsquery('monetary value')
ORDER BY rank DESC;
```

---

## **Insert Example**

```sql
INSERT INTO term_search_index (term_id, content, tsv)
VALUES (
  'b8fc423d-97ab-4b16-afe7-2f9a39eb785c',
  'Gross Domestic Product (GDP) is a monetary measure of the market value of all the final goods and services produced in a specific time period.',
  to_tsvector('english', 'Gross Domestic Product (GDP) is a monetary measure of the market value of all the final goods and services produced in a specific time period.')
);
```

---