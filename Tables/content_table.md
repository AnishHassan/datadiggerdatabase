# Table: `content_table`

---

# Description

Stores various content items (such as articles, reports, documents, or media) associated with specific countries. Each entry includes metadata such as title, description, categorization (category/subcategory), and a reference URL. Commonly used in dashboards, feeds, or content-driven interfaces where country-level filtering and categorization are essential.

---

# Schema

| Column Name    | Data Type     | Null | Constraints                                | Description                                       |
| -------------- | ------------- | ---- | ------------------------------------------ | ------------------------------------------------- |
| `id`           | INT4          | NO   | Primary Key, Auto-Increment                | Unique identifier for the content record          |
| `country_iso2` | BPCHAR(2)     | NO   | Indexed (`idx_content_table_country_iso2`) | ISO Alpha-2 country code (e.g., `US`, `IN`, `BR`) |
| `title`        | VARCHAR(255)  | NO   | -                                          | Title of the content                              |
| `description`  | TEXT          | YES  | -                                          | Brief summary or abstract of the content          |
| `content_url`  | VARCHAR(2083) | YES  | -                                          | Direct URL to the content resource                |
| `created_at`   | TIMESTAMP     | NO   | Default: `CURRENT_TIMESTAMP`               | Timestamp when the content was created            |
| `updated_at`   | TIMESTAMP     | NO   | Default: `CURRENT_TIMESTAMP`               | Timestamp of the last content update              |
| `category`     | VARCHAR(255)  | YES  | -                                          | Optional high-level classification of content     |
| `subcategory`  | VARCHAR(255)  | YES  | -                                          | Optional refined classification under `category`  |

---

# Indexes

| Index Name                       | Columns        | Type  | Description                         |
| -------------------------------- | -------------- | ----- | ----------------------------------- |
| `idx_content_table_country_iso2` | `country_iso2` | BTREE | Optimizes filtering by country code |

---

# Relationships

* `country_iso2` may conceptually relate to foreign keys in country-based tables (e.g., `economic_indicator_metadata.country_iso2`, `countries.iso2`), though no strict FK is enforced for flexibility.
* Can be linked to `charts`, `dashboards`, or `insights` via tags, filters, or categories for dynamic rendering of contextual content.

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

Country-Specific Dashboards : Display curated content relevant to selected countries.
Content Feed or Blog : Dynamically render the latest or trending items by region or category.
Analytics : Generate summary statistics by category, time, or region.
Contextual Add-ons : Provide relevant articles alongside charts or data points.

---

# Query Examples

# 1. Fetch Latest Content for a Specific Country

```sql
SELECT *
FROM content_table
WHERE country_iso2 = 'US'
ORDER BY created_at DESC
LIMIT 5;
```

---

# 2. Group Content by Category for Analytics

```sql
SELECT category, COUNT(*) AS total
FROM content_table
GROUP BY category
ORDER BY total DESC;
```

---

# 3. Retrieve Articles Tagged as Forecasts

```sql
SELECT title, content_url
FROM content_table
WHERE subcategory ILIKE '%forecast%'
ORDER BY created_at DESC;
```

---

# Insert Example

```sql
INSERT INTO content_table (
  country_iso2, title, description, content_url,
  category, subcategory
) VALUES (
  'US',
  'U.S. Economic Outlook 2025',
  'An in-depth look at projected GDP growth and inflation trends.',
  'https://example.com/us-economic-outlook-2025',
  'Economy',
  'Forecast'
);
```