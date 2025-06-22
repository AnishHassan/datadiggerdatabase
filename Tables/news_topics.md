# Table: `news_topics`

## Description

Stores a list of unique topics used to categorize or tag news items across the system. Enables better organization, filtering, and discoverability of news content.

---

## Schema

| Column Name | Data Type | Null | Default             | Constraints | Description                                      |
| ----------- | --------- | ---- | ------------------- | ----------- | ------------------------------------------------ |
| id          | UUID      | NO   | `gen_random_uuid()` | Primary Key | Unique identifier for the news topic             |
| name        | TEXT      | NO   | -                   | Unique      | Name of the topic (e.g., "Inflation", "Markets") |

---

## Business Rules

* Each topic `name` must be unique.
* Topic names should be concise and descriptive for user-facing tagging.

---

## Indexes

| Index Name             | Column | Type  | Description                                 |
| ---------------------- | ------ | ----- | ------------------------------------------- |
| `news_topics_pkey`     | id     | BTREE | Primary key for fast retrieval via topic ID |
| `news_topics_name_key` | name   | BTREE | Ensures topic names remain unique           |

---

## Example Row

```json
{
  "id": "02f7ac64-d4a5-4bdf-8a61-df51057d7a89",
  "name": "Monetary Policy"
}
```