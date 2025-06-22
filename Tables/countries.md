# Table: `countries`

### Description:

Stores detailed information about recognized countries, including standardized codes, geopolitical details, currency, and national symbols. This table supports associations with country-specific events, calendars, economic indicators, and financial instruments.

---

## Schema

| Column Name           | Data Type    | Null | Constraints            | Description                                                 |
| --------------------- | ------------ | ---- | ---------------------- | ----------------------------------------------------------- |
| id                    | INT4         | NO   | Primary Key, Auto-incr | Unique identifier for the country                           |
| country_long_name     | VARCHAR(100) | NO   | -                      | Full name of the country (e.g., *United States of America*) |
| country_short_name    | VARCHAR(100) | NO   | -                      | Short or common name (e.g., *United States*)                |
| country_iso2          | BPCHAR(2)    | NO   | UNIQUE                 | ISO 3166-1 alpha-2 code (e.g., *US*, *FR*)                  |
| country_iso3          | BPCHAR(3)    | NO   | UNIQUE                 | ISO 3166-1 alpha-3 code (e.g., *USA*, *FRA*)                |
| currency_short        | BPCHAR(3)    | YES  | -                      | ISO 4217 currency code (e.g., *USD*, *EUR*)                 |
| currency_long         | VARCHAR(100) | YES  | -                      | Full currency name (e.g., *United States Dollar*)           |
| capital_city          | VARCHAR(100) | YES  | -                      | Capital of the country (e.g., *Washington, D.C.*)           |
| region                | VARCHAR(100) | YES  | -                      | Geographical region (e.g., *Americas*, *Europe*)            |
| sub_region            | VARCHAR(100) | YES  | -                      | Sub-region (e.g., *Northern America*, *Southern Europe*)    |
| blocs                 | JSONB        | YES  | -                      | Economic/political blocs (e.g., `["G7", "NATO"]`)           |
| country_iso_3166_1    | INT4         | YES  | -                      | Numeric ISO 3166-1 code (e.g., *840* for the US)            |
| cenbank_longname      | VARCHAR(150) | YES  | -                      | Full name of the central bank                               |
| cenbank_shortname     | VARCHAR(50)  | YES  | -                      | Abbreviation or common name for the central bank            |
| national_flag_path    | VARCHAR(255) | YES  | -                      | Path or URL to the national flag image                      |
| state_flag_path       | VARCHAR(255) | YES  | -                      | Path or URL to a state or regional flag image               |
| city_flag_path        | VARCHAR(255) | YES  | -                      | Path or URL to a city flag image                            |

---

## Notes:

* The `country_iso2` field is a key foreign reference in many other tables (`auction_calendar_events`, `bond_auction_results`, `commodity_calendar_events`, etc.).
* The `blocs` field allows for flexible storage of multi-affiliation data (e.g., EU, G20, ASEAN).
* Flag paths can point to internal file paths or external URLs depending on how the system handles media.

---

## Example Row

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

## Query Examples

**Get all countries in the European Union:**

```sql
SELECT country_short_name, country_iso2
FROM countries
WHERE blocs @> '["EU"]';
```

**Retrieve ISO codes and currencies:**

```sql
SELECT country_iso2, country_iso3, currency_short, currency_long
FROM countries
ORDER BY country_short_name;
```

**Search countries by region:**

```sql
SELECT country_short_name, region, sub_region
FROM countries
WHERE region = 'Asia';
```

