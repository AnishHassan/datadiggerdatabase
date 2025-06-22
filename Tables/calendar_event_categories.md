# Table: `calendar_event_categories`

# Description**

A **junction table** that defines a many-to-many relationship between economic calendar events and their associated categories. Each record links one event to one category, allowing an event to belong to multiple categories and vice versa.

---

# Schema

| Column Name   | Data Type | Null | Constraints                                 | Description                                  |
| ------------- | --------- | ---- | ------------------------------------------- | -------------------------------------------- |
| `event_id`    | INT4      | NO   | Foreign Key → `economic_calendar_events.id` | Unique identifier of the calendar event      |
| `category_id` | INT4      | NO   | Foreign Key → `calendar_categories.id`      | Unique identifier of the associated category |

---

# Indexes

| Index Name                                  | Columns                     | Type  | Description                                      |
| ------------------------------------------- | --------------------------- | ----- | ------------------------------------------------ |
| `calendar_event_categories_pkey`            | (`event_id`, `category_id`) | BTREE | Composite primary key to enforce uniqueness      |
| `idx_calendar_event_categories_category_id` | `category_id`               | BTREE | Improves lookup speed for category-based queries |

---

# Relationships

* `event_id` →  `economic_calendar_events.id`
* `category_id` →  `calendar_categories.id`

---

# Usage Scenarios

* Assigning multiple thematic categories (e.g., "Inflation", "GDP") to a single economic event.
* Building filterable dashboards based on event categories.
* Supporting complex event-tagging for search, alerts, and analysis.

---

# Example Record

```json
{
  "event_id": 2051,
  "category_id": 12
}
```

---

# Query Examples

# Get all categories for a given event:

```sql
SELECT c.name
FROM calendar_event_categories ec
JOIN calendar_categories c ON ec.category_id = c.id
WHERE ec.event_id = 2051;
```

---

# Get all events linked to a category:

```sql
SELECT e.*
FROM calendar_event_categories ec
JOIN economic_calendar_events e ON ec.event_id = e.id
WHERE ec.category_id = 12;
```

---

# Insert Example

```sql
INSERT INTO calendar_event_categories (event_id, category_id)
VALUES (2051, 12);
```
