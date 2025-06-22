# Table: `glossary_term_tags`

---

## **Description**

Join table that links glossary terms to tags, enabling a many-to-many relationship. This structure allows flexible categorization and filtering of glossary entries using a dynamic tagging system.

---

## **Schema**

| Column Name | Data Type | Null | Default | Constraints | Description                                       |
| ----------- | --------- | ---- | ------- | ----------- | ------------------------------------------------- |
| `term_id`   | UUID      | NO   | –       | Foreign Key | References `glossary_terms.id`; the tagged term   |
| `tag_id`    | UUID      | NO   | –       | Foreign Key | References `tags.id`; the tag applied to the term |

---

## **Relationships**

| Related Table    | Relationship Type | Foreign Key | Description                                      |
| ---------------- | ----------------- | ----------- | ------------------------------------------------ |
| `glossary_terms` | Many-to-One       | `term_id`   | Associates the tag with a specific glossary term |
| `tags`           | Many-to-One       | `tag_id`    | Associates the term with a specific tag          |

---

## **Business Rules**

* A tag can only be associated once per glossary term.
* Both `term_id` and `tag_id` must exist in their respective parent tables.
* Acts as the canonical way to manage term-tag relationships in the system.

---

## **Indexes**

| Index Name                | Column                | Type  | Description                                        |
| ------------------------- | --------------------- | ----- | -------------------------------------------------- |
| `glossary_term_tags_pkey` | (`term_id`, `tag_id`) | BTREE | Composite primary key ensuring uniqueness per pair |

---

## **Example Record**

```json
{
  "term_id": "dbd97b2c-5a36-465a-8c51-1e8c3fa09fcb",
  "tag_id": "0ff84e78-f723-4d66-9c62-773f2033b3cf"
}
```

---

## **Usage Scenarios**

* Categorizing glossary terms by topics such as "macroeconomics", "finance", "AI", etc.
* Enabling search or filtering of glossary terms by assigned tags.
* Supporting dynamic faceted browsing or tagging-based recommendation systems.

---

## **Query Examples**

### Get all tags for a given glossary term

```sql
SELECT t.name
FROM tags t
JOIN glossary_term_tags gtt ON gtt.tag_id = t.id
WHERE gtt.term_id = 'dbd97b2c-5a36-465a-8c51-1e8c3fa09fcb';
```

---

### Get all glossary terms associated with a given tag

```sql
SELECT gt.term
FROM glossary_terms gt
JOIN glossary_term_tags gtt ON gtt.term_id = gt.id
WHERE gtt.tag_id = '0ff84e78-f723-4d66-9c62-773f2033b3cf';
```

---

## **Insert Example**

```sql
INSERT INTO glossary_term_tags (term_id, tag_id)
VALUES (
  'dbd97b2c-5a36-465a-8c51-1e8c3fa09fcb',
  '0ff84e78-f723-4d66-9c62-773f2033b3cf'
);
```