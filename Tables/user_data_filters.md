# Table: `user_data_filters`

## Description

Stores saved data filter configurations tied to a user's preference profile. This allows for persistent filtering criteria across views such as dashboards, feeds, or reports.

---

## Schema

| Column Name    | Data Type | Null | Default | Constraints | Description                                                             |
| -------------- | --------- | ---- | ------- | ----------- | ----------------------------------------------------------------------- |
| id             | UUID      | NO   | -       | Primary Key | Unique identifier for the data filter set                               |
| preference_id  | UUID      | YES  | -       | Foreign Key | Links to a `user_preferences` record for association                    |
| region         | TEXT      | YES  | -       |             | Region-based filter (e.g., "North America", "Asia-Pacific")             |
| industry       | TEXT      | YES  | -       |             | Industry filter (e.g., "Finance", "Healthcare")                         |
| category       | TEXT      | YES  | -       |             | General category filter (e.g., "News", "Reports")                       |
| filters_json   | JSONB     | YES  | -       |             | Arbitrary structured filters (e.g., {"source": "WSJ", "score": ">0.8"}) |
| created_at     | TIMESTAMP | YES  | `now()` |             | Timestamp when the filter configuration was created                     |

---

## Relationships

| Related Table      | Relationship Type | Foreign Key     | Description                                      |
| ------------------ | ----------------- | --------------- | ------------------------------------------------ |
| `user_preferences` | Many-to-One       | `preference_id` | Each filter set belongs to one preference record |

---

## Business Rules

* Each user preference can be associated with multiple data filter sets.
* `filters_json` supports complex, nested filtering logic.
* UI should allow users to save, edit, or delete these filters dynamically.

---

## Indexes

| Index Name               | Column         | Type  | Description                                         |
| ------------------------ | -------------- | ----- | --------------------------------------------------- |
| `user_data_filters_pkey` | id             | BTREE | Primary key to uniquely identify the filter set     |
| (optional)               | preference_id  | BTREE | Useful for quick lookup of filters per user profile |

---

## Example Row

```json
{
  "id": "adbf5d5e-9a24-46de-80cf-32f4ff87b4c7",
  "preference_id": "aa8b92a0-99c7-41c3-9cd3-f5f8130e0e5b",
  "region": "Europe",
  "industry": "Technology",
  "category": "News",
  "filters_json": {
    "min_score": 0.7,
    "source": ["Reuters", "Bloomberg"],
    "topics": ["AI", "Startups"]
  },
  "created_at": "2025-06-21T11:22:45.000Z"
}
```