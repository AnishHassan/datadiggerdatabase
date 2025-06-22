# Table: `auction_categories`

### Description:

Defines categories for auction calendar events, enabling grouping and classification of different types of financial instruments or auction formats.

---

## Schema

| Column Name | Data Type    | Null | Default             | Constraints      | Description                                     |
| ----------- | ------------ | ---- | ------------------- | ---------------- | ----------------------------------------------- |
| id          | INT4         | NO   | auto-increment      | Primary Key      | Unique identifier for each auction category     |
| name        | VARCHAR(255) | NO   | -                   | Unique, Not Null | Name of the category (e.g., "Government Bonds") |
| created_at  | TIMESTAMPTZ  | NO   | `CURRENT_TIMESTAMP` | -                | Timestamp when the category was created         |
| updated_at  | TIMESTAMPTZ  | NO   | `CURRENT_TIMESTAMP` | -                | Timestamp when the category was last updated    |

---

## Notes

* Used to categorize auction events in the `auction_calendar_events` table via the `category_id` foreign key.
* Useful for organizing and filtering auctions by predefined categories like "Treasury Bills", "Corporate Bonds", etc.
* Keep `updated_at` current with a trigger or update logic for accurate modification tracking.

---

## Suggested Indexes

| Index Name                    | Columns | Type  | Purpose                      |
| ----------------------------- | ------- | ----- | ---------------------------- |
| `auction_categories_pkey`     | id      | BTREE | Primary key                  |
| `auction_categories_name_key` | name    | BTREE | Ensure unique category names |

---

## Example Row

```json
{
  "id": 1,
  "name": "Government Bonds",
  "created_at": "2025-06-01T10:00:00Z",
  "updated_at": "2025-06-20T08:30:00Z"
}
```

---

## Query Examples

**List all available auction categories:**

```sql
SELECT id, name
FROM auction_categories
ORDER BY name;
```

**Find category by name:**

```sql
SELECT *
FROM auction_categories
WHERE name = 'Corporate Bonds';
```

---

## Insert Example

```sql
INSERT INTO auction_categories (name)
VALUES ('Corporate Bonds');
```