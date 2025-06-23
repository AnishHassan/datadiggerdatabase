# Table: `user_data_filters`

### **Description**

Persists **user-specific filter configurations** for personalized experiences across dashboards, feeds, and reports. Enables **custom views**, **saved queries**, and **preloaded filter states** within an application.

---

## **Schema Overview**

| Column Name     | Data Type | Null | Default | Constraints | Description                                                      |
| --------------- | --------- | ---- | ------- | ----------- | ---------------------------------------------------------------- |
| `id`            | UUID      | NO   | —       | Primary Key | Uniquely identifies a saved filter configuration                 |
| `preference_id` | UUID      | YES  | —       | Foreign Key | Links to `user_preferences` for user-level association           |
| `region`        | TEXT      | YES  | —       |             | Optional region-specific scope (e.g., "Europe", "Global")        |
| `industry`      | TEXT      | YES  | —       |             | Optional industry tag (e.g., "Healthcare", "Finance")            |
| `category`      | TEXT      | YES  | —       |             | Optional category or module (e.g., "News", "Reports")            |
| `filters_json`  | JSONB     | YES  | —       |             | Nested or dynamic filtering criteria (e.g., `{"score": ">0.9"}`) |
| `created_at`    | TIMESTAMP | YES  | `now()` |             | Timestamp of creation, defaults to current time                  |

---

## Relationships

| Related Table      | Relationship Type | Foreign Key     | Description                                                  |
| ------------------ | ----------------- | --------------- | ------------------------------------------------------------ |
| `user_preferences` | Many-to-One       | `preference_id` | Each filter set is tied to a user’s saved preference profile |

---

## Business Logic

* A **user** (via `user_preferences`) can **store multiple filter sets** for various use cases.
* `filters_json` enables flexible, schema-less filter logic suitable for faceted search, advanced filtering, or query caching.
* Ideal for **saving UI state**, allowing users to return to the same view/filter combo across sessions.

---

## Common `filters_json` Examples

```json
// Example 1: Range + tag-based filtering
{
  "score": { "gt": 0.8 },
  "tags": ["AI", "Startups"],
  "source": ["Reuters", "Bloomberg"]
}

// Example 2: Date and source filters
{
  "published_after": "2025-01-01",
  "source": "WSJ"
}
```

---

## Index Suggestions

| Index Name                 | Column          | Type  | Description                              |
| -------------------------- | --------------- | ----- | ---------------------------------------- |
| `user_data_filters_pkey`   | `id`            | BTREE | Uniquely identifies each filter set      |
| `idx_data_filters_pref_id` | `preference_id` | BTREE | Optimizes lookup of all filters per user |
| *(optional)*               | `created_at`    | BTREE | For sorting/filtering filters by recency |

---

## Example Record

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

---

## Query Examples

**Get all saved filters for a user's preferences:**

```sql
SELECT * FROM user_data_filters
WHERE preference_id = 'aa8b92a0-99c7-41c3-9cd3-f5f8130e0e5b'
ORDER BY created_at DESC;
```

**Search filters targeting a specific industry and category:**

```sql
SELECT * FROM user_data_filters
WHERE industry = 'Finance' AND category = 'Reports';
```

---

## Notes on Data Security & Usage

* `filters_json` should **not include PII** or security-sensitive data.
* Design front-end UIs to **validate and sanitize JSON structures** before persisting.
* Consider **versioning** or **naming filter sets** if filters need to be reused or referenced by name.

---