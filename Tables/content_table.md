# Table: `content_table`

# **Description**

Stores various pieces of content (such as articles, documents, or media) associated with countries. Each entry contains metadata including title, description, and optional categorization.

---

# Schema

| Column Name    | Data Type     | Null | Constraints                                | Description                              |
| -------------- | ------------- | ---- | ------------------------------------------ | ---------------------------------------- |
| `id`           | INT4          | NO   | Primary Key, Auto-Increment                | Unique identifier for the content record |
| `country_iso2` | BPCHAR(2)     | NO   | Indexed (`idx_content_table_country_iso2`) | ISO Alpha-2 country code                 |
| `title`        | VARCHAR(255)  | NO   | -                                          | Title of the content                     |
| `description`  | TEXT          | YES  | -                                          | Brief content description                |
| `content_url`  | VARCHAR(2083) | YES  | -                                          | Link to the full content or resource     |
| `created_at`   | TIMESTAMP     | NO   | Default: `CURRENT_TIMESTAMP`               | Timestamp of creation                    |
| `updated_at`   | TIMESTAMP     | NO   | Default: `CURRENT_TIMESTAMP`               | Timestamp of last update                 |
| `category`     | VARCHAR(255)  | YES  | -                                          | Optional high-level category             |
| `subcategory`  | VARCHAR(255)  | YES  | -                                          | Optional more specific subcategory       |

---

# Indexes

| Index Name                       | Columns        | Type  | Description                    |
| -------------------------------- | -------------- | ----- | ------------------------------ |
| `idx_content_table_country_iso2` | `country_iso2` | BTREE | Optimizes filtering by country |

---

# Relationships

* `country_iso2` may relate to country codes used in other tables (e.g., `economic_indicator_metadata.country_iso2`), though no FK constraint is enforced here.

---

# Example Record

```json
{
  "id": 78,
  "country_iso2": "US",
  "title": "U.S. Economic Outlook 2025",
  "description": "An in-depth look at projected GDP growth and inflation trends.",
  "content_url": "https://example.com/us-economic-outlook-2025",
  "created_at": "2025-06-22 10:30:00",
  "updated_at": "2025-06-22 10:30:00",
  "category": "Economy",
  "subcategory": "Forecast"
}
```

---

# Usage Scenarios

* Displaying country-specific economic or news content.
* Organizing content for dashboards or feeds.
* Supporting content filtering by category, subcategory, or country.

---

# Query Examples

# Fetch latest content for a specific country:

```sql
SELECT *
FROM content_table
WHERE country_iso2 = 'US'
ORDER BY created_at DESC
LIMIT 5;
```

# Group content by category for analytics:

```sql
SELECT category, COUNT(*) as total
FROM content_table
GROUP BY category
ORDER BY total DESC;
```