# Table: `bond_auction_results`

### Description:

Stores outcome data for government bond auctions, including yield statistics, bid coverage, and accepted amounts. Typically linked to auction events from the `auction_calendar_events` table.

---

## Schema

| Column Name        | Data Type    | Null | Default             | Constraints | Description                                    |
| ------------------ | ------------ | ---- | ------------------- | ----------- | ---------------------------------------------- |
| id                 | INT4         | NO   | -                   | Primary Key | Unique identifier for the auction result       |
| country_iso2       | BPCHAR(2)    | YES  | -                   | -           | Two-letter ISO country code (e.g., `US`, `DE`) |
| auction_date       | DATE         | YES  | -                   | -           | Date the auction was held                      |
| instrument         | VARCHAR(64)  | YES  | -                   | -           | Name or identifier of the auctioned instrument |
| maturity_date      | DATE         | YES  | -                   | -           | Maturity date of the bond                      |
| amount_offered     | INT8         | YES  | -                   | -           | Total amount of bonds offered in the auction   |
| amount_accepted    | INT8         | YES  | -                   | -           | Total amount accepted from bids                |
| bid_to_cover       | NUMERIC(5,2) | YES  | -                   | -           | Ratio of bids received to bids accepted        |
| avg_yield          | NUMERIC(6,3) | YES  | -                   | -           | Average yield of accepted bids                 |
| highest_yield      | NUMERIC(6,3) | YES  | -                   | -           | Highest yield accepted                         |
| lowest_yield       | NUMERIC(6,3) | YES  | -                   | -           | Lowest yield accepted                          |
| auction_event_id   | INT4         | YES  | -                   | Foreign Key | Optional link to `auction_calendar_events(id)` |
| created_at         | TIMESTAMPTZ  | NO   | `CURRENT_TIMESTAMP` | -           | Timestamp when this record was created         |
| updated_at         | TIMESTAMPTZ  | NO   | `CURRENT_TIMESTAMP` | -           | Timestamp when this record was last updated    |

---

## Notes

* `bid_to_cover` is a key metric used to gauge auction demand.
* `avg_yield`, `highest_yield`, and `lowest_yield` are central performance indicators for investors and analysts.
* `auction_event_id` can help join results with scheduled auction details.

---

## Suggested Indexes

| Index Name                      | Columns                        | Type  | Purpose                                         |
| ------------------------------- | ------------------------------ | ----- | ----------------------------------------------- |
| `bond_auction_results_pkey`     | id                             | BTREE | Primary key for row uniqueness                  |
| `idx_bond_auction_country_date` | (country_iso2, auction_date)   | BTREE | Fast queries by country and auction date        |
| `idx_bond_auction_event_id`     | auction_event_id               | BTREE | Join performance with `auction_calendar_events` |

---

## Example Row

```json
{
  "id": 4523,
  "country_iso2": "DE",
  "auction_date": "2025-06-15",
  "instrument": "10Y Bund",
  "maturity_date": "2035-06-15",
  "amount_offered": 5000000000,
  "amount_accepted": 4500000000,
  "bid_to_cover": 1.25,
  "avg_yield": 2.145,
  "highest_yield": 2.210,
  "lowest_yield": 2.080,
  "auction_event_id": 207,
  "created_at": "2025-06-15T12:30:00Z",
  "updated_at": "2025-06-15T12:30:00Z"
}
```

---

## Query Examples

**Get all auctions in a specific country in 2025:**

```sql
SELECT *
FROM bond_auction_results
WHERE country_iso2 = 'DE'
  AND auction_date BETWEEN '2025-01-01' AND '2025-12-31';
```

**Find auctions with high demand (bid-to-cover > 2):**

```sql
SELECT *
FROM bond_auction_results
WHERE bid_to_cover > 2.0
ORDER BY auction_date DESC;
```

---

## Insert Example

```sql
INSERT INTO bond_auction_results (
  country_iso2, auction_date, instrument, maturity_date,
  amount_offered, amount_accepted, bid_to_cover,
  avg_yield, highest_yield, lowest_yield, auction_event_id
) VALUES (
  'DE', '2025-06-15', '10Y Bund', '2035-06-15',
  5000000000, 4500000000, 1.25,
  2.145, 2.210, 2.080, 207
);
```