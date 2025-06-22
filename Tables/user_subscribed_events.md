# Table: `user_subscribed_events`

# Description
Keeps track of which users have subscribed to specific calendar events for update notifications. This feature allows users to receive alerts or follow activity on events they are interested in — such as economic events.

---

# Schema

| Column Name   | Data Type | Null | Default           | Constraints   | Description                                                       |
|---------------|-----------|------|-------------------|---------------|-------------------------------------------------------------------|
| id            | INT       | NO   | -                 | Primary Key   | Unique identifier for the user-event subscription                |
| user_id       | UUID      | YES  | NULL              | Foreign Key   | The user who is following the event                               |
| event_id      | UUID      | YES  | NULL              | Foreign Key   | The calendar event being followed                                 |
| status        | VARCHAR   | YES  | NULL              |               | Current follow status (e.g., `active`, `unsubscribed`)            |
| subscribed_at | TIMESTAMP | NO   | CURRENT_TIMESTAMP |               | Timestamp of when the follow/subscription was made                |

---

# Relationships

| Related Table | Relationship Type | Foreign Key | Description                                 |
|---------------|-------------------|-------------|---------------------------------------------|
| users         | Many-to-One       | user_id     | A user can follow many events               |
| events        | Many-to-One       | event_id    | An event can be followed by many users      |

---

# Business Rules

- A user can only follow a given event once — enforce with a unique constraint on `(user_id, event_id)`.
- If a user unfollows an event, either delete the row or update the `status` to `unsubscribed`.
- This is for **notification and engagement**, not related to payments or RSVPs.

---

# Suggested Indexes

| Index Name                         | Column    | Type   | Description                                     |
|------------------------------------|-----------|--------|-------------------------------------------------|
| `idx_user_subscribed_events_user`  | user_id   | BTREE  | For retrieving all events a user is subscribed to |
| `idx_user_subscribed_events_event` | event_id  | BTREE  | For checking all followers of an event           |

---

# Example Row

```json
{
  "id": 214,
  "user_id": "da75b7b2-3e9c-4ac3-8f9d-0112f946af58",
  "event_id": "a14249e1-bf44-4f91-afe1-c35b4e64cf98",
  "status": "active",
  "subscribed_at": "2025-06-20T10:15:00Z"
}
