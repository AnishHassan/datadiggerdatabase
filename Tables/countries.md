# Table: `countries`

---

## Description**

Stores standardized and detailed information for recognized countries. This includes ISO codes, naming conventions, currency, geopolitical regions, national symbols, and affiliations (e.g., blocs). The table supports joins with various domain-specific tables such as economic indicators, financial events, and calendar systems.

---

# Schema

| Column Name          | Data Type    | Null | Constraints            | Description                                                |
| -------------------- | ------------ | ---- | ---------------------- | ---------------------------------------------------------- |
| `id`                 | INT4         | NO   | Primary Key, Auto-Incr | Unique internal identifier for the country                 |
| `country_long_name`  | VARCHAR(100) | NO   | -                      | Official long-form name (e.g., *United States of America*) |
| `country_short_name` | VARCHAR(100) | NO   | -                      | Common short name (e.g., *United States*)                  |
| `country_iso2`       | BPCHAR(2)    | NO   | UNIQUE                 | ISO 3166-1 alpha-2 code (e.g., *US*, *DE*)                 |
| `country_iso3`       | BPCHAR(3)    | NO   | UNIQUE                 | ISO 3166-1 alpha-3 code (e.g., *USA*, *DEU*)               |
| `currency_short`     | BPCHAR(3)    | YES  | -                      | ISO 4217 currency code (e.g., *USD*, *EUR*)                |
| `currency_long`      | VARCHAR(100) | YES  | -                      | Full currency name (e.g., *United States Dollar*)          |
| `capital_city`       | VARCHAR(100) | YES  | -                      | Capital city (e.g., *Washington, D.C.*)                    |
| `region`             | VARCHAR(100) | YES  | -                      | Geographical region (e.g., *Americas*, *Europe*)           |
| `sub_region`         | VARCHAR(100) | YES  | -                      | Sub-region (e.g., *Northern America*)                      |
| `blocs`              | JSONB        | YES  | -                      | Economic/political blocs as array (e.g., `["G7", "OECD"]`) |
| `country_iso_3166_1` | INT4         | YES  | -                      | ISO 3166-1 numeric code (e.g., *840*)                      |
| `cenbank_longname`   | VARCHAR(150) | YES  | -                      | Full name of the central bank                              |
| `cenbank_shortname`  | VARCHAR(50)  | YES  | -                      | Common name or abbreviation of the central bank            |
| `national_flag_path` | VARCHAR(255) | YES  | -                      | Path or URL to national flag image                         |
| `state_flag_path`    | VARCHAR(255) | YES  | -                      | Path or URL to state/regional flag                         |
| `city_flag_path`     | VARCHAR(255) | YES  | -                      | Path or URL to city/municipal flag                         |

---

# Relationships

* `country_iso2` is commonly referenced in other tables, such as:

  * `content_table.country_iso2`
  * `economic_indicator_metadata.country_iso2`
  * `auction_calendar_events.country_iso2`
  * `bond_auction_results.country_iso2`
  * `commodity_calendar_events.country_iso2`
* Used as a central join point for country-specific aggregations.
* No enforced foreign key constraints, but relational joins are supported via `country_iso2`.

---

# Example Record

```json
{
  "id": 1,
  "country_long_name": "United States of America",
  "country_short_name": "United States",
  "country_iso2": "US",
  "country_iso3": "USA",
  "currency_short": "USD",
  "currency_long": "United States Dollar",
  "capital_city": "Washington, D.C.",
  "region": "Americas",
  "sub_region": "Northern America",
  "blocs": ["G7", "G20", "OECD"],
  "country_iso_3166_1": 840,
  "cenbank_longname": "Board of Governors of the Federal Reserve System",
  "cenbank_shortname": "Federal Reserve",
  "national_flag_path": "/flags/us/national.svg",
  "state_flag_path": null,
  "city_flag_path": null
}
```

---

# Usage Scenarios

* Populate dropdowns or selection filters for country-specific UIs.
* Join with content or economic data using `country_iso2` for rich visualizations.
* Filter or group data by region, bloc, or currency for dashboard analytics.
* Map rendering with associated flags or central bank metadata.
* Multi-affiliation bloc analysis (e.g., all G7 members or ASEAN members).

---

# Query Examples

# 1. Get all countries in the European Union:

```sql
SELECT country_short_name, country_iso2
FROM countries
WHERE blocs @> '["EU"]';
```

---

# 2. Retrieve ISO codes and currency info:

```sql
SELECT country_iso2, country_iso3, currency_short, currency_long
FROM countries
ORDER BY country_short_name;
```

---

# 3. Search countries by region:

```sql
SELECT country_short_name, region, sub_region
FROM countries
WHERE region = 'Asia';
```

---

# 4. Get countries with a specific central bank name:

```sql
SELECT country_short_name, cenbank_shortname
FROM countries
WHERE cenbank_shortname ILIKE '%Reserve%';
```

---

# Insert Example

```sql
INSERT INTO countries (
  country_long_name,
  country_short_name,
  country_iso2,
  country_iso3,
  currency_short,
  currency_long,
  capital_city,
  region,
  sub_region,
  blocs,
  country_iso_3166_1,
  cenbank_longname,
  cenbank_shortname,
  national_flag_path,
  state_flag_path,
  city_flag_path
) VALUES (
  'United States of America',
  'United States',
  'US',
  'USA',
  'USD',
  'United States Dollar',
  'Washington, D.C.',
  'Americas',
  'Northern America',
  '["G7", "G20", "OECD"]',
  840,
  'Board of Governors of the Federal Reserve System',
  'Federal Reserve',
  '/flags/us/national.svg',
  NULL,
  NULL
);
```