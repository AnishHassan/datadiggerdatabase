# Table: `news_feed`

## Description

Captures summarized news articles and alerts, often shorter and more digestible than full news wire entries. Includes metadata such as publication time, associated region, source reference, and a trending flag.

---

## Schema

| Column Name   | Data Type | Null | Default             | Constraints | Description                                                  |
| ------------- | --------- | ---- | ------------------- | ----------- | ------------------------------------------------------------ |
| id            | UUID      | NO   | `gen_random_uuid()` | Primary Key | Unique identifier for the news feed entry                    |
| publish_date  | DATE      | NO   | -                   |             | The date the news feed was published                         |
| publish_time  | TIME      | NO   | -                   |             | The time the news feed was published                         |
| headline      | TEXT      | NO   | -                   |             | Title or main line of the news                               |
| summary       | TEXT      | YES  | NULL                |             | Optional short summary of the news content                   |
| link          | TEXT      | YES  | NULL                |             | Optional external link to full content or original source    |
| is_trending   | BOOLEAN   | YES  | `false`             |             | Marks whether the item is currently trending                 |
| region_id     | INT       | YES  | NULL                | Foreign Key | Optionally links the news to a region in the `regions` table |
| source_id     | INT       | YES  | NULL                | Foreign Key | Optionally links to the `news_sources` table for attribution |
| created_at    | TIMESTAMP | YES  | `CURRENT_TIMESTAMP` |             | Timestamp when the record was created                        |

---

## Relationships

| Related Table  | Relationship Type | Foreign Key | Description                                          |
| -------------- | ----------------- | ----------- | ---------------------------------------------------- |
| `regions`      | Many-to-One       | region_id   | Optionally associates a news feed item with a region |
| `news_sources` | Many-to-One       | source_id   | Identifies the original source of the news feed item |

---

## Business Rules

* `headline`, `publish_date`, and `publish_time` are mandatory.
* If `source_id` or `region_id` is provided, they must reference valid IDs in their respective tables.
* The `is_trending` flag helps surface high-priority or currently popular items.

---

## Indexes

| Index Name                      | Column                         | Type  | Description                                       |
| ------------------------------- | ------------------------------ | ----- | ------------------------------------------------- |
| `news_feed_pkey`                | id                             | BTREE | Primary key for unique identification             |
| `idx_publish_date_publish_time` | (publish_date, publish_time)   | BTREE | Could improve chronological filtering and sorting |

---

## Example Row

```json
{
  "id": "c1f7d9a2-3df8-4b20-84b4-96c8dc823e02",
  "publish_date": "2025-06-21",
  "publish_time": "09:30:00",
  "headline": "Global Markets Rise Ahead of Fed Meeting",
  "summary": "Stocks climb as investors anticipate signals on future interest rates.",
  "link": "https://example.com/markets-rise-fed",
  "is_trending": true,
  "region_id": 1,
  "source_id": 2,
  "created_at": "2025-06-21T09:30:00Z"
}
```