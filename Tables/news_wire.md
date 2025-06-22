# Table: `news_wire`

## Description

Stores real-time news entries, typically related to financial markets, economic developments, or relevant events. Each entry includes timestamped metadata, classification, source attribution, and optional linkage to regional and event data.

---

## Schema

| Column Name        | Data Type | Null | Default             | Constraints | Description                                                        |
| ------------------ | --------- | ---- | ------------------- | ----------- | ------------------------------------------------------------------ |
| id                 | UUID      | NO   | `gen_random_uuid()` | Primary Key | Unique identifier for the news entry                               |
| news_date          | DATE      | NO   | -                   |             | The calendar date when the news was reported                       |
| news_time          | TIME      | NO   | -                   |             | The exact time the news was published                              |
| headline           | TEXT      | NO   | -                   |             | Main headline text of the news                                     |
| news_type          | TEXT      | NO   | -                   |             | Type or category of the news (e.g., breaking, macro, policy, etc.) |
| source             | TEXT      | NO   | -                   |             | Source of the news (e.g., Bloomberg, Reuters)                      |
| source_logo        | TEXT      | YES  | NULL                |             | Optional URL pointing to the logo of the source                    |
| region_id          | INT       | YES  | NULL                | Foreign Key | Links to a region in the `regions` table                           |
| link               | TEXT      | YES  | NULL                |             | External link to the full news article                             |
| related_event_id   | UUID      | YES  | NULL                | Foreign Key | References an event in the `calendar_events` table (if applicable) |

---

## Relationships

| Related Table     | Relationship Type | Foreign Key        | Description                                                  |
| ----------------- | ----------------- | ------------------ | ------------------------------------------------------------ |
| `regions`         | Many-to-One       | `region_id`        | Connects news to a geographic region                         |
| `calendar_events` | Many-to-One       | `related_event_id` | Optionally links a news article to a specific calendar event |

---

## Business Rules

* `headline`, `news_type`, and `source` are required for each record.
* `news_date` and `news_time` must always be present for accurate chronological ordering.
* If `related_event_id` is provided, it must reference a valid `calendar_events.id`.
* `region_id` can help filter or group news geographically.

---

## Indexes

| Index Name                | Column                   | Type  | Description                                          |
| ------------------------- | ------------------------ | ----- | ---------------------------------------------------- |
| `news_wire_pkey`          | id                       | BTREE | Primary key for unique identification                |
| `idx_news_wire_date_time` | (news_date, news_time)   | BTREE | Could optimize sorting and filtering chronologically |

---

## Example Row

```json
{
  "id": "ae2b340e-d81d-4f87-94b3-b1f05e8971d4",
  "news_date": "2025-07-01",
  "news_time": "13:45:00",
  "headline": "ECB Hints at Rate Hike Amid Inflation Pressure",
  "news_type": "monetary_policy",
  "source": "Reuters",
  "source_logo": "https://example.com/logos/reuters.png",
  "region_id": 2,
  "link": "https://reuters.com/article/ecb-policy-news",
  "related_event_id": "f1a6d8c2-4fdc-4921-a9e4-5a1c9cf251aa"
}
```