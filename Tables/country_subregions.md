# Table: `country_subregions`

---

# Description

Defines administrative or geographical subdivisions (e.g., states, provinces, territories) within a country. Enables fine-grained localization for content, filters, data segmentation, or UI displays that go below the country level.

---

# Schema

| Column Name      | Data Type    | Null | Constraints                                     | Description                                                      |
| ---------------- | ------------ | ---- | ----------------------------------------------- | ---------------------------------------------------------------- |
| `id`             | INT4         | NO   | Primary Key, Auto-Increment                     | Unique identifier for each subregion                             |
| `country_iso2`   | BPCHAR(2)    | NO   | Indexed (`idx_country_subregions_country_iso2`) | ISO 3166-1 Alpha-2 code of the parent country                    |
| `subregion_name` | VARCHAR(100) | NO   | -                                               | Name of the subregion (e.g., *California*, *Bavaria*)            |
| `subregion_code` | VARCHAR(10)  | YES  | -                                               | Abbreviated code or symbol (e.g., *CA*, *BY*)                    |
| `display_order`  | INT4         | YES  | Default: `0`                                    | Optional integer used for sorting subregions in consistent order |

---

# Indexes

| Index Name                            | Columns        | Type  | Description                                 |
| ------------------------------------- | -------------- | ----- | ------------------------------------------- |
| `idx_country_subregions_country_iso2` | `country_iso2` | BTREE | Accelerates subregion lookups by country ID |

---

# Relationships

* `country_iso2` should match ISO codes from the `countries` table.
* Can be used to associate with:

  * `content_table.country_iso2` for regional content targeting
  * `languages.country_iso2` for language localization
  * `economic_indicator_metadata.country_iso2` for deeper breakdowns

---

# Example Record

```json
{
  "id": 12,
  "country_iso2": "US",
  "subregion_name": "California",
  "subregion_code": "CA",
  "display_order": 1
}
```

---

# Usage Scenarios

* Displaying or filtering content at the subnational level (e.g., news, reports, stats).
* Organizing UIs such as dropdowns, maps, or autocomplete inputs by region.
* Supporting geo-targeted delivery for economic, demographic, or policy data.
* Structuring backend hierarchies for applications that operate regionally.

---

# Query Examples

# 1. List all subregions for a specific country:

```sql
SELECT subregion_name, subregion_code
FROM country_subregions
WHERE country_iso2 = 'US'
ORDER BY display_order ASC;
```

---

# 2. Count number of subregions per country:

```sql
SELECT country_iso2, COUNT(*) AS total_subregions
FROM country_subregions
GROUP BY country_iso2
ORDER BY total_subregions DESC;
```

---

# 3. Get subregion codes for a given region name (partial match):

```sql
SELECT subregion_code
FROM country_subregions
WHERE subregion_name ILIKE '%York%';
```

---

# Insert Example**

```sql
INSERT INTO country_subregions (
  country_iso2,
  subregion_name,
  subregion_code,
  display_order
) VALUES (
  'US',
  'California',
  'CA',
  1
);
```