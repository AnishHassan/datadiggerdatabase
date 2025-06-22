# Table: `economic_calendar_events`

### Description:

This table records scheduled economic events and data releases (e.g., GDP reports, unemployment figures) by country and region. These events are key indicators used in financial markets to assess the economic outlook and affect trading decisions.

---

## Schema

| Column Name       | Data Type    | Null | Constraints                  | Description                                                               |
| ----------------- | ------------ | ---- | ---------------------------- | ------------------------------------------------------------------------- |
| id                | INT4         | NO   | Primary Key, Auto-incr       | Unique identifier for the economic event                                  |
| indicator_code    | VARCHAR(32)  | YES  | -                            | Code representing the economic indicator (e.g., *GDP\_QOQ*, *CPI*)        |
| country_iso2      | BPCHAR(2)    | YES  | FK → `countries`             | ISO 3166-1 alpha-2 code for the country (e.g., *US*, *DE*)                |
| source_shortname  | VARCHAR(32)  | YES  | -                            | Short name for the source/publisher of the data (e.g., *BLS*, *Eurostat*) |
| region            | VARCHAR(64)  | YES  | -                            | Region or economic area (e.g., *Eurozone*, *North America*)               |
| date              | DATE         | NO   | -                            | Date of the event or release                                              |
| release_for       | DATE         | YES  | -                            | Period that the data refers to (e.g., Q1 2025 for a GDP release)          |
| time              | TIME         | YES  | -                            | Scheduled time of release (local or UTC depending on `timezone`)          |
| timezone          | VARCHAR(64)  | YES  | DEFAULT: `'UTC'`             | Timezone of the event time                                                |
| title             | VARCHAR(255) | YES  | -                            | Descriptive title of the event (e.g., *GDP YoY Final*)                    |
| impact            | VARCHAR(6)   | YES  | DEFAULT: `'Low'`             | Subjective assessment of market impact (e.g., *Low*, *Medium*, *High*)    |
| parent_event_id   | INT4         | YES  | FK → self                    | Links to a parent event (e.g., revisions or grouped indicators)           |
| created_at        | TIMESTAMP    | YES  | DEFAULT: `CURRENT_TIMESTAMP` | Timestamp when this record was created                                    |

---

## Notes

* `country_iso2` allows joining with the `countries` table for full geopolitical context.
* `indicator_code` links conceptually to indicators like GDP, CPI, Unemployment Rate, etc. (this may be normalized into a separate `indicators` table in a more relational schema).
* `parent_event_id` enables grouping of related events, such as preliminary/final versions or regional rollups.
* `impact` is often used in filtering for user alerts and economic newsfeeds.

---

## Example Record

```json
{
  "id": 12453,
  "indicator_code": "GDP_QOQ",
  "country_iso2": "US",
  "source_shortname": "BEA",
  "region": "North America",
  "date": "2025-07-30",
  "release_for": "2025-06-30",
  "time": "08:30:00",
  "timezone": "America/New_York",
  "title": "Gross Domestic Product (QoQ) Final",
  "impact": "High",
  "parent_event_id": null,
  "created_at": "2025-06-21 14:22:11"
}
```

---

## Query Examples

**List high-impact events for the US in July 2025:**

```sql
SELECT title, date, time, impact
FROM economic_calendar_events
WHERE country_iso2 = 'US'
  AND impact = 'High'
  AND date BETWEEN '2025-07-01' AND '2025-07-31'
ORDER BY date, time;
```

**Join with `countries` to get full country names:**

```sql
SELECT e.title, e.date, c.country_long_name
FROM economic_calendar_events e
JOIN countries c ON e.country_iso2 = c.country_iso2
WHERE e.impact = 'Medium'
ORDER BY e.date;
```