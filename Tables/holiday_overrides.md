# Table: `holiday_overrides`

### **Description**

This table defines manual overrides for scheduled economic data releases that fall on holidays or require adjustment. It allows altering the default release schedule (e.g., moving it earlier or later) by specifying a new release date and time.

---

## Schema

| Column Name          | Data Type    | Null | Constraints                  | Description                                                                |
| -------------------- | ------------ | ---- | ---------------------------- | -------------------------------------------------------------------------- |
| `id`                 | INT4         | NO   | Primary Key, Auto-Increment  | Unique identifier for each override entry                                  |
| `feed_id`            | INT4         | NO   | –                            | Identifier referencing the original data feed (not explicitly foreign key) |
| `release_date`       | DATE         | NO   | –                            | New scheduled release date                                                 |
| `release_time_local` | TIME         | YES  | Default: `'13:00:00'`        | Local time of release on the override date                                 |
| `cron_expression`    | VARCHAR(255) | YES  | –                            | Optional CRON syntax to describe release schedule pattern                  |
| `holiday_name`       | VARCHAR(255) | YES  | –                            | Name of the holiday or reason for the override                             |
| `created_at`         | TIMESTAMP    | YES  | Default: `CURRENT_TIMESTAMP` | Timestamp when this override record was created                            |

---

## Usage Scenarios

* Adjusting economic data releases when national holidays affect standard schedules.
* Automating scheduling systems with exception rules.
* Supporting custom feeds or API responses where release schedules deviate from norms.

---

## Example Record

```json
{
  "id": 17,
  "feed_id": 201,
  "release_date": "2025-12-25",
  "release_time_local": "10:00:00",
  "cron_expression": null,
  "holiday_name": "Christmas Day",
  "created_at": "2025-11-15 08:30:00"
}
```

---

## Query Examples

### Get all upcoming overrides:

```sql
SELECT * 
FROM holiday_overrides
WHERE release_date >= CURRENT_DATE
ORDER BY release_date;
```

### Find overrides for a specific feed:

```sql
SELECT *
FROM holiday_overrides
WHERE feed_id = 201
ORDER BY release_date DESC;
```