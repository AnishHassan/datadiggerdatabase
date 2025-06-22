# Table: `user_activity_feed`

# Description
Tracks all major user-triggered and system-generated activities for auditing, notification, and engagement purposes. Each row represents a single event related to user interaction or account lifecycle.

---

# Schema

| Column Name     | Data Type     | Null | Default           | Constraints               | Description                                                   |
|-----------------|---------------|------|-------------------|----------------------------|---------------------------------------------------------------|
| id              | int           | NO   | -                 | PK, Auto-increment         | Unique identifier for the activity record                     |
| user_id         | uuid          | YES  | -                 | FK → users(id)             | The user associated with the activity                         |
| activity_type   | varchar(100)  | YES  | -                 |                            | Type of activity (e.g., login, dataset_saved)                 |
| reference_id    | uuid          | YES  | -                 |                            | Optional ID pointing to related entity (chart, dataset, etc.) |
| message         | text          | YES  | -                 |                            | Human-readable activity message                               |
| status          | varchar(30)   | YES  | -                 |                            | Status like success, failed, pending                          |
| created_at      | timestamp     | NO   | CURRENT_TIMESTAMP |                            | Timestamp when activity occurred                              |


# Relationships

| Related Table | Relationship Type | Foreign Key | Description                                      |
|---------------|-------------------|-------------|--------------------------------------------------|
| users         | Many-to-One       | user_id     | Activities are tied to the user who triggered them |

---

# Business Rules

- Activities should be logged for major user events (login, logout, password change, saving a chart or dataset, subscription update, etc.).
- `reference_id` helps to trace the activity to its corresponding object if needed.
- `message` should be clear, actionable, and translatable for UI display.

---

# Common `activity_type` Values

- `login`
- `logout`
- `password_changed`
- `subscription_updated`
- `chart_saved`
- `event_subscribed`
- `dataset_uploaded`

---

# Example Row

```json
{
  "id": 1257,
  "user_id": "bfe2e0e7-1234-4d9c-881a-782e0f72ae7a",
  "activity_type": "dataset_uploaded",
  "reference_id": "a3e91234-cb56-44ab-bf79-d891b3af9812",
  "message": "Uploaded a new COVID-19 dataset for VIC region",
  "status": "success",
  "created_at": "2025-06-21T10:42:11Z"
}
