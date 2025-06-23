# Table: `tags`

### **Description**

Stores unique tags used to categorize or label various entities (such as news articles, events, or glossary terms) across the platform. Tags improve content discoverability, filtering, and contextual grouping.

---

## **Schema**

| Column Name | Data Type | Null | Constraints                               | Description                                                        |
| ----------- | --------- | ---- | ----------------------------------------- | ------------------------------------------------------------------ |
| `id`        | UUID      | NO   | Primary Key, Default: `gen_random_uuid()` | Unique identifier for each tag                                     |
| `name`      | TEXT      | NO   | Unique                                    | Human-readable tag label or keyword (e.g., `inflation`, `climate`) |

---

## **Relationships**

| Related Table | Relationship Type | Description                                                |
| ------------- | ----------------- | ---------------------------------------------------------- |
| `entity_tags` | One-to-Many       | Maps tags to various entities (e.g., news, glossary, etc.) |
| `news_tags`   | One-to-Many       | Links tags to news articles                                |
| `event_tags`  | One-to-Many       | Associates tags with events                                |

> Tag usage is normalized via mapping tables to support many-to-many relationships with tagged entities.

---

## **Example Record**

```json
{
  "id": "dc8b7e5b-63a7-489b-8d77-32ef8a1c26d6",
  "name": "inflation"
}
```

---

## **Usage Scenarios**

* Assign tags to content types like news, events, or glossary entries.
* Enable users to filter or search content based on tags.
* Group related content items under shared topical labels.
* Power tag-based recommendations or related-content modules.

---

## **Query Examples**

### Find all tag names (alphabetical):

```sql
SELECT name
FROM tags
ORDER BY name ASC;
```

### Get tag by name:

```sql
SELECT *
FROM tags
WHERE name = 'climate';
```

### Count how many tags exist:

```sql
SELECT COUNT(*) AS total_tags
FROM tags;
```

---

## **Insert Example**

```sql
INSERT INTO tags (name)
VALUES ('climate');
```

---