# Table: `newsfeed_topics`

## Description

Maps individual newsfeed items to one or more topics. This join table enables a many-to-many relationship between news content and topical tags, improving categorization and discovery.

---

## Schema

| Column Name  | Data Type | Null | Default | Constraints | Description                                      |
| ------------ | --------- | ---- | ------- | ----------- | ------------------------------------------------ |
| newsfeed_id | UUID       | NO   | -       | Foreign Key | References a news entry in the `news_feed` table |
| topic_id    | UUID       | NO   | -       | Foreign Key | References a topic in the `news_topics` table    |

---

## Relationships

| Related Table | Relationship Type | Foreign Key  | Description                                        |
| ------------- | ----------------- | ------------ | -------------------------------------------------- |
| `news_feed`   | Many-to-Many      | newsfeed_id  | Connects newsfeed items to topical classifications |
| `news_topics` | Many-to-Many      | topic_id     | Associates topics to newsfeed entries              |

---

## Business Rules

* Each `(newsfeed_id, topic_id)` pair must be unique.
* A single newsfeed entry can have multiple topics.
* A single topic can be linked to many newsfeed entries.

---

## Indexes

| Index Name             | Column                    | Type  | Description                                           |
| ---------------------- | ------------------------- | ----- | ----------------------------------------------------- |
| `newsfeed_topics_pkey` | (newsfeed_id, topic_id)   | BTREE | Composite primary key ensuring uniqueness per mapping |

---

## Example Row

```json
{
  "newsfeed_id": "af513d1e-1d4a-4336-b229-48c52182e813",
  "topic_id": "3b1e89ff-d1c1-442f-9476-b9e2302f94a2"
}
```