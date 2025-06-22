# Table: `glossary_term_links`

---

## **Description**

Stores internal or external resource links associated with glossary terms. These links can guide users to additional reading materials, official definitions, or related educational content—enhancing the glossary’s usefulness and depth.

---

## **Schema**

| Column Name | Data Type | Null | Default             | Constraints | Description                                                   |
| ----------- | --------- | ---- | ------------------- | ----------- | ------------------------------------------------------------- |
| `id`        | UUID      | NO   | `gen_random_uuid()` | Primary Key | Unique identifier for each reference link                     |
| `term_id`   | UUID      | YES  | NULL                | Foreign Key | References `glossary_terms.id`; can be null for general links |
| `title`     | TEXT      | NO   | –                   | –           | Descriptive title of the link (e.g., name of the article)     |
| `url`       | TEXT      | NO   | –                   | –           | Fully qualified URL (should be well-formed, preferably HTTPS) |

---

## **Relationships**

| Related Table    | Relationship Type | Foreign Key | Description                                                |
| ---------------- | ----------------- | ----------- | ---------------------------------------------------------- |
| `glossary_terms` | Many-to-One       | `term_id`   | Associates one or more links with a specific glossary term |

---

## **Business Rules**

* Both `title` and `url` are mandatory and must be non-empty.
* `term_id` is optional to support general-purpose educational links.
* URLs should be valid and ideally use HTTPS.
* No duplicate `url` entries for the same `term_id` to avoid redundancy.

---

## **Indexes**

| Index Name                 | Column | Type  | Description                         |
| -------------------------- | ------ | ----- | ----------------------------------- |
| `glossary_term_links_pkey` | `id`   | BTREE | Ensures fast lookup via primary key |

---

## **Example Record**

```json
{
  "id": "d23fa9c3-f1b2-4b33-9d5f-4a678ae11b5a",
  "term_id": "8bcf108d-cf5e-42e2-9e89-01353e94fc60",
  "title": "Investopedia: GDP Definition",
  "url": "https://www.investopedia.com/terms/g/gdp.asp"
}
```

---

## **Usage Scenarios**

* Enhancing glossary terms with references to trusted external resources.
* Providing internal documentation or wiki links for company-specific terms.
* Supporting learning paths and onboarding flows with curated learning materials.
* Allowing glossary terms to link to video tutorials, blog posts, or APIs.

---

## **Query Examples**

### Fetch all links for a given glossary term

```sql
SELECT title, url
FROM glossary_term_links
WHERE term_id = '8bcf108d-cf5e-42e2-9e89-01353e94fc60';
```

---

### Fetch unassigned (general) glossary links

```sql
SELECT *
FROM glossary_term_links
WHERE term_id IS NULL;
```

---

## **Insert Example**

```sql
INSERT INTO glossary_term_links (term_id, title, url)
VALUES (
  '8bcf108d-cf5e-42e2-9e89-01353e94fc60',
  'Investopedia: GDP Definition',
  'https://www.investopedia.com/terms/g/gdp.asp'
);
```