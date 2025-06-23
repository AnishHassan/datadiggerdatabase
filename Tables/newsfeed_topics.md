# Table: `newsfeed_topics`

---

## Description

Join table supporting a **many-to-many relationship** between `news_feed` entries and `news_topics`.
This mapping allows **flexible tagging** of news content with multiple topical labels to improve **content classification, relevance, and discoverability**.

---

## Schema

| Column Name   | Data Type | Null | Default | Constraints | Description                                             |
| ------------- | --------- | ---- | ------- | ----------- | ------------------------------------------------------- |
| `newsfeed_id` | UUID      | NO   | –       | Foreign Key | References a news entry in the `news_feed` table (`id`) |
| `topic_id`    | UUID      | NO   | –       | Foreign Key | References a topic in the `news_topics` table (`id`)    |

---

## Relationships

| Related Table | Relationship Type | Foreign Key   | Description                                                  |
| ------------- | ----------------- | ------------- | ------------------------------------------------------------ |
| `news_feed`   | Many-to-Many      | `newsfeed_id` | Each topic mapping links back to a specific newsfeed item    |
| `news_topics` | Many-to-Many      | `topic_id`    | Enables tagging each news item with multiple relevant topics |

---

## Business Rules

* **Composite primary key**: (`newsfeed_id`, `topic_id`) to prevent duplicate mappings.
* A single `newsfeed_id` can be linked to **multiple** `topic_id`s.
* A single `topic_id` can be linked to **multiple** `newsfeed_id`s.

---

## Indexes

| Index Name             | Column(s)                   | Type  | Description                                           |
| ---------------------- | --------------------------- | ----- | ----------------------------------------------------- |
| `newsfeed_topics_pkey` | (`newsfeed_id`, `topic_id`) | BTREE | Ensures unique mappings and improves join performance |

---

## Example Record

```json
{
  "newsfeed_id": "af513d1e-1d4a-4336-b229-48c52182e813",
  "topic_id": "3b1e89ff-d1c1-442f-9476-b9e2302f94a2"
}
```

---

## Usage Scenarios

* Tagging news stories with current themes (e.g., *Elections*, *Climate Change*, *AI*)
* Filtering or recommending articles based on trending or user-preferred topics
* Powering personalized topic-based feeds or analytics dashboards

---

## Query Examples

### Get All Topics for a Newsfeed Item

```sql
SELECT t.name
FROM newsfeed_topics nt
JOIN news_topics t ON nt.topic_id = t.id
WHERE nt.newsfeed_id = 'af513d1e-1d4a-4336-b229-48c52182e813';
```

### Find All News Items Tagged with a Specific Topic

```sql
SELECT nf.*
FROM newsfeed_topics nt
JOIN news_feed nf ON nf.id = nt.newsfeed_id
WHERE nt.topic_id = '3b1e89ff-d1c1-442f-9476-b9e2302f94a2';
```

---