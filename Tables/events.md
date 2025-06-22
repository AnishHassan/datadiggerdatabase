# Table: `events`

### Description:

Stores information about various events, including regional or thematic activities, relevant to the platform's content.

---

## Schema

| Column Name | Data Type | Null | Default | Constraints | Description                                   |
| ----------- | --------- | ---- | ------- | ----------- | --------------------------------------------- |
| id          | UUID      | NO   | -       | Primary Key | Unique identifier for the event               |
| title       | TEXT      | YES  | -       | -           | Title or name of the event                    |
| description | TEXT      | YES  | -       | -           | Detailed description or summary of the event  |
| region      | TEXT      | YES  | -       | -           | Geographical region associated with the event |
| type        | TEXT      | YES  | -       | -           | Classification or category of the event       |
| date_range  | DATERANGE | YES  | -       | -           | Start and end dates for the event             |
| created_at  | TIMESTAMP | YES  | `now()` | -           | Timestamp when the event was recorded         |

---

## Notes

* `date_range` allows flexible storage of events spanning multiple days.
* `type` can be filtered for event types like `webinar`, `conference`, `launch`, etc.
* You can create indexes on `region`, `type`, or `date_range` if you're running frequent searches.

---

## Suggested Indexes

| Index Name          | Columns     | Type  | Purpose                                  |
| ------------------- | ----------- | ----- | ---------------------------------------- |
| `events_pkey`       | id          | BTREE | Primary key                              |
| `events_region_idx` | region      | BTREE | Search/filter events by region           |
| `events_type_idx`   | type        | BTREE | Search/filter events by type             |
| `events_date_idx`   | date\_range | GIST  | Efficient querying by date range overlap |

---

## Example Row

```json
{
  "id": "49d12b2f-2222-41ce-b8ac-4e21b323cc93",
  "title": "AI Regulation Summit",
  "description": "Summit to discuss ethical frameworks for AI governance.",
  "region": "EU",
  "type": "Conference",
  "date_range": "[2025-10-12,2025-10-14]",
  "created_at": "2025-06-21T15:42:00Z"
}
```

# Query Example

SELECT * FROM events
WHERE region = 'EU'
  AND lower(date_range) >= CURRENT_DATE
ORDER BY lower(date_range);

# Find events of type 'Webinar' in the next month

SELECT * FROM events
WHERE type = 'Webinar'
  AND date_range && daterange(CURRENT_DATE, CURRENT_DATE + INTERVAL '1 month');


# Insert example data

INSERT INTO events (id, title, description, region, type, date_range)
VALUES (
  gen_random_uuid(),
  'AI Regulation Summit',
  'Summit to discuss ethical frameworks for AI governance.',
  'EU',
  'Conference',
  daterange('2025-10-12', '2025-10-14', '[]')
);
