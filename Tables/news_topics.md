# Table: `news_topics`

---

## Description

Stores a centralized list of unique topics that are used to categorize or tag news items. This promotes structured classification, improved filtering, and enhanced discoverability of content throughout the platform.

---

## Schema

| Column Name | Data Type | Null | Default             | Constraints | Description                                         |
| ----------- | --------- | ---- | ------------------- | ----------- | --------------------------------------------------- |
| `id`        | UUID      | NO   | `gen_random_uuid()` | Primary Key | Unique identifier for the news topic                |
| `name`      | TEXT      | NO   | -                   | Unique      | Descriptive label for the topic (e.g., "Inflation") |

---

## Relationships

| Referencing Table             | Foreign Key | Relationship | Description                                              |
| ----------------------------- | ----------- | ------------ | -------------------------------------------------------- |
| `news_feed_topics` (junction) | `topic_id`  | Many-to-Many | Used to associate news feed entries with multiple topics |

> *Topics are typically linked through a many-to-many junction table (e.g., `news_feed_topics`).*

---

## Business Rules

* Topic names must be **unique** and **non-null**.
* Topic names should be **concise**, **descriptive**, and suitable for display in user-facing UI elements.

---

## Indexes

| Index Name             | Column | Type  | Description                                       |
| ---------------------- | ------ | ----- | ------------------------------------------------- |
| `news_topics_pkey`     | `id`   | BTREE | Ensures unique identification of each topic       |
| `news_topics_name_key` | `name` | BTREE | Enforces uniqueness and speeds up lookups by name |

---

## Example Record

```json
{
  "id": "02f7ac64-d4a5-4bdf-8a61-df51057d7a89",
  "name": "Monetary Policy"
}
```

---

## Usage Scenarios

* **Tagging**: Assign multiple topics to a single news item for better contextual grouping.
* **Filtering**: Allow users to browse or subscribe to news by topic.
* **Recommendation**: Suggest related news based on shared topics.

---

## Query Examples

### Get All Topics

```sql
SELECT * FROM news_topics
ORDER BY name ASC;
```

### Search Topics by Keyword

```sql
SELECT id, name
FROM news_topics
WHERE name ILIKE '%policy%';
```

### Find Topics for a Given News Feed Entry

(Assuming junction table `news_feed_topics`)

```sql
SELECT nt.id, nt.name
FROM news_feed_topics nft
JOIN news_topics nt ON nft.topic_id = nt.id
WHERE nft.news_feed_id = 'c1f7d9a2-3df8-4b20-84b4-96c8dc823e02';
```

---

## Insert Example

```sql
INSERT INTO news_topics (id, name)
VALUES (
  '02f7ac64-d4a5-4bdf-8a61-df51057d7a89',
  'Monetary Policy'
);
```