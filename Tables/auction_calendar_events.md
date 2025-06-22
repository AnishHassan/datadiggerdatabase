# Table: `auction_calendar_events`

### Description:

Stores scheduled auction-related events by country and region, including details like date, time, instrument, impact level, and maturity.

---

## Schema

| Column Name       | Data Type    | Null | Default             | Constraints | Description                                                  |
| ----------------- | ------------ | ---- | ------------------- | ----------- | ------------------------------------------------------------ |
| id                | INT4         | NO   | auto-increment      | Primary Key | Unique identifier for each auction event                     |
| country_iso2      | BPCHAR(2)    | YES  | -                   | -           | ISO 3166-1 alpha-2 country code (e.g., 'US', 'DE')           |
| region            | VARCHAR(64)  | YES  | -                   | -           | Region within the country                                    |
| event_date        | DATE         | YES  | -                   | -           | Date of the auction event                                    |
| event_time        | TIME         | YES  | -                   | -           | Time of the auction event                                    |
| instrument        | VARCHAR(64)  | YES  | -                   | -           | Instrument being auctioned (e.g., bonds, treasury bills)     |
| event_type        | TEXT         | YES  | `'Offer'`           | -           | Type of auction event (e.g., 'Offer', 'Reopen', etc.)        |
| title             | VARCHAR(255) | YES  | -                   | -           | Descriptive title of the event                               |
| impact            | VARCHAR(16)  | YES  | -                   | -           | Impact level (e.g., 'High', 'Medium', 'Low')                 |
| parent_event_id   | INT4         | YES  | -                   | -           | Reference to a parent auction event (if any)                 |
| created_at        | TIMESTAMP    | YES  | `CURRENT_TIMESTAMP` | -           | Timestamp when the event was created                         |
| maturity_date     | DATE         | YES  | -                   | -           | Maturity date of the instrument being auctioned              |
| category_id       | INT4         | YES  | -                   | -           | Foreign key reference to event category (if applicable)      |
| auction_datetime  | TIMESTAMP    | YES  | -                   | -           | Combined date and time of the auction for sorting or display |
| issue_number      | VARCHAR(50)  | YES  | -                   | -           | Identifier for the specific instrument issuance              |

---

## Notes

* `country_iso2` + `event_date` index supports fast filtering by country and date.
* `auction_datetime` index allows efficient ordering or filtering by full event timestamp.
* You may link `parent_event_id` to another row in this table for hierarchical relationships.

---

## Suggested Indexes

| Index Name                     | Columns                      | Type  | Purpose                                          |
| ------------------------------ | ---------------------------- | ----- | ------------------------------------------------ |
| `auction_calendar_events_pkey` | id                           | BTREE | Primary key                                      |
| `idx_auction_country_date`     | (country_iso2, event_date)   | BTREE | Filter/search auction events by country and date |
| `idx_auction_event_datetime`   | auction_datetime             | BTREE | Filter/sort by auction datetime                  |

---

## Example Row

```json
{
  "id": 1012,
  "country_iso2": "US",
  "region": "Federal",
  "event_date": "2025-07-01",
  "event_time": "13:00:00",
  "instrument": "10Y Treasury Bond",
  "event_type": "Offer",
  "title": "10-Year Treasury Note Auction",
  "impact": "High",
  "parent_event_id": null,
  "created_at": "2025-06-21T12:30:00Z",
  "maturity_date": "2035-07-01",
  "category_id": 3,
  "auction_datetime": "2025-07-01T13:00:00Z",
  "issue_number": "US10Y-2035-07"
}
```

---

## Query Examples

**Get all auction events for Germany in July 2025:**

```sql
SELECT *
FROM auction_calendar_events
WHERE country_iso2 = 'DE'
  AND event_date BETWEEN '2025-07-01' AND '2025-07-31';
```

**Find high-impact upcoming auctions:**

```sql
SELECT *
FROM auction_calendar_events
WHERE impact = 'High'
  AND auction_datetime > CURRENT_TIMESTAMP
ORDER BY auction_datetime;
```

---

## Insert Example

```sql
INSERT INTO auction_calendar_events (
  country_iso2, region, event_date, event_time, instrument,
  event_type, title, impact, maturity_date, category_id,
  auction_datetime, issue_number
) VALUES (
  'FR', 'National', '2025-07-05', '11:00:00', '5Y Bond',
  'Offer', '5-Year Government Bond Auction', 'Medium', '2030-07-05',
  2, '2025-07-05 11:00:00', 'FR5Y-2030-07'
);
```