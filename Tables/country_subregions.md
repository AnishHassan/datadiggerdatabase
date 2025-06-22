# Table: `country_subregions`

### **Description**

Defines named subregions (such as states, provinces, or territories) within a country. Useful for organizing localized data or content below the country level.

---

## Schema

| Column Name      | Data Type    | Null | Constraints                                     | Description                                               |
| ---------------- | ------------ | ---- | ----------------------------------------------- | --------------------------------------------------------- |
| `id`             | INT4         | NO   | Primary Key, Auto-Increment                     | Unique identifier for the subregion                       |
| `country_iso2`   | BPCHAR(2)    | NO   | Indexed (`idx_country_subregions_country_iso2`) | ISO Alpha-2 code of the country this subregion belongs to |
| `subregion_name` | VARCHAR(100) | NO   | -                                               | Name of the subregion (e.g., "California", "Bavaria")     |
| `subregion_code` | VARCHAR(10)  | YES  | -                                               | Short code or abbreviation (e.g., "CA", "BY")             |
| `display_order`  | INT4         | YES  | Default: `0`                                    | Optional order value for sorting subregions               |

---

## Indexes

| Index Name                            | Columns        | Type  | Description                               |
| ------------------------------------- | -------------- | ----- | ----------------------------------------- |
| `idx_country_subregions_country_iso2` | `country_iso2` | BTREE | Optimizes lookup of subregions by country |

---

## Relationships

* `country_iso2` is expected to match ISO 3166-1 Alpha-2 codes, possibly used in other tables like `content_table`, `languages`, or `economic_indicator_metadata`.

---

## Example Record

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

## Usage Scenarios

* Displaying localized content or statistics by subregion.
* Mapping regions within a country in UIs (e.g., dropdowns, filters).
* Supporting administrative or geographic breakdowns for data.

---

## Query Examples

### List all subregions for a specific country:

```sql
SELECT subregion_name, subregion_code
FROM country_subregions
WHERE country_iso2 = 'US'
ORDER BY display_order ASC;
```

### Count number of subregions per country:

```sql
SELECT country_iso2, COUNT(*) AS total_subregions
FROM country_subregions
GROUP BY country_iso2
ORDER BY total_subregions DESC;
```
