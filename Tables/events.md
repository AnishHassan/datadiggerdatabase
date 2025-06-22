# Table: `events`

### **Description**

This table stores details of various scheduled or unscheduled events, such as conferences, webinars, regional summits, or platform-related activities. Events are categorized by type, region, and time span using a flexible date range field, enabling efficient filtering and overlap querying for analytics and user-facing interfaces.

---

## **Schema**

| Column Name   | Data Type | Null | Default | Constraints | Description                                                |
| ------------- | --------- | ---- | ------- | ----------- | ---------------------------------------------------------- |
| `id`          | UUID      | NO   | –       | Primary Key | Unique identifier for the event                            |
| `title`       | TEXT      | YES  | –       | –           | Title or name of the event                                 |
| `description` | TEXT      | YES  | –       | –           | Detailed description or summary of the event               |
| `region`      | TEXT      | YES  | –       | –           | Geographical region associated with the event (e.g., "EU") |
| `type`        | TEXT      | YES  | –       | –           | Category/type of event (e.g., "Webinar", "Conference")     |
| `date_range`  | DATERANGE | YES  | –       | –           | Start and end dates of the event as a range                |
| `created_at`  | TIMESTAMP | YES  | `now()` | –           | Timestamp when the record was created                      |

---

## **Relationships**

* No foreign keys, but `region` and `type` can be linked to supporting lookup/reference tables if normalization is needed.
* Can be joined with user engagement tables, calendars, or event attendance records (if modeled separately).

---

## **Example Record**

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

---

## **Usage Scenarios**

* Displaying upcoming or historical events on a user dashboard or timeline
* Filtering events by region, category, or time window
* Powering alerts or notifications for events starting soon
* Performing overlap analysis to see which events occur during major economic activities
* Supporting editorial, newsfeed, or announcement content

---

## **Query Examples**

### Find future events in the EU region

```sql
SELECT * FROM events
WHERE region = 'EU'
  AND lower(date_range) >= CURRENT_DATE
ORDER BY lower(date_range);
```

---

### Find all webinars happening in the next 30 days

```sql
SELECT * FROM events
WHERE type = 'Webinar'
  AND date_range && daterange(CURRENT_DATE, CURRENT_DATE + INTERVAL '1 month');
```

---

### List all conferences happening this year

```sql
SELECT * FROM events
WHERE type = 'Conference'
  AND date_range && daterange(date_trunc('year', CURRENT_DATE), date_trunc('year', CURRENT_DATE) + INTERVAL '1 year');
```

---

## **Insert Example**

```sql
INSERT INTO events (id, title, description, region, type, date_range)
VALUES (
  gen_random_uuid(),
  'AI Regulation Summit',
  'Summit to discuss ethical frameworks for AI governance.',
  'EU',
  'Conference',
  daterange('2025-10-12', '2025-10-14', '[]')
);
```