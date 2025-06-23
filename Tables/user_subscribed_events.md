# Table: `user_subscribed_events`

---

### Description

Tracks user subscriptions to specific calendar events in order to deliver update notifications. Enables users to follow economic, financial, or community events they are interested in, helping improve engagement and awareness.

---

### Schema

| Column Name    | Data Type | Null | Default            | Constraints            | Description                                          |
| -------------- | --------- | ---- | ------------------ | ---------------------- | ---------------------------------------------------- |
| id             | INT       | NO   | -                  | Primary Key            | Unique identifier for this user-event subscription   |
| user_id        | UUID      | NO   | -                  | Foreign Key → `users`  | ID of the user who subscribed to the event           |
| event_id       | UUID      | NO   | -                  | Foreign Key → `events` | ID of the calendar event being followed              |
| status         | VARCHAR   | YES  | 'active'           |                        | Subscription status (`active`, `unsubscribed`, etc.) |
| subscribed_at  | TIMESTAMP | NO   | CURRENT_TIMESTAMP  |                        | Timestamp when the subscription was created          |

---

### Relationships

| Related Table | Relationship Type | Foreign Key | Description                                |
| ------------- | ----------------- | ----------- | ------------------------------------------ |
| `users`       | Many-to-One       | user_id     | A user can follow multiple events          |
| `events`      | Many-to-One       | event_id    | An event can be followed by multiple users |

---

### Example Record

```json
{
  "id": 214,
  "user_id": "da75b7b2-3e9c-4ac3-8f9d-0112f946af58",
  "event_id": "a14249e1-bf44-4f91-afe1-c35b4e64cf98",
  "status": "active",
  "subscribed_at": "2025-06-20T10:15:00Z"
}
```

---

### Usage Scenarios

* **User notification system**: Notify users when followed events are updated or about to occur.
* **Event popularity insights**: Measure event interest by counting followers.
* **Unsubscription tracking**: Keep history of who followed/unfollowed events.
* **Personal calendar feed**: Display all events a user is currently subscribed to.

---

### Query Examples

#### Get all active subscriptions for a user

```sql
SELECT * 
FROM user_subscribed_events
WHERE user_id = 'da75b7b2-3e9c-4ac3-8f9d-0112f946af58'
  AND status = 'active';
```

---

#### Count how many users are following an event

```sql
SELECT COUNT(*) 
FROM user_subscribed_events
WHERE event_id = 'a14249e1-bf44-4f91-afe1-c35b4e64cf98'
  AND status = 'active';
```

---

#### Unsubscribe a user from an event

```sql
UPDATE user_subscribed_events
SET status = 'unsubscribed'
WHERE user_id = 'da75b7b2-3e9c-4ac3-8f9d-0112f946af58'
  AND event_id = 'a14249e1-bf44-4f91-afe1-c35b4e64cf98';
```

---

### Insert Example

```sql
INSERT INTO user_subscribed_events (
  user_id,
  event_id,
  status,
  subscribed_at
) VALUES (
  'da75b7b2-3e9c-4ac3-8f9d-0112f946af58',
  'a14249e1-bf44-4f91-afe1-c35b4e64cf98',
  'active',
  CURRENT_TIMESTAMP
);
```

---

### Business Rules

* **Unique Constraint** on `(user_id, event_id)` should be enforced to prevent duplicate subscriptions.
* If unsubscribed, either update `status = 'unsubscribed'` or delete the row (depends on audit requirements).
* Designed purely for **notifications and tracking**, not for event RSVPs or ticketing.

---

### Suggested Indexes

| Index Name                         | Column    | Type  | Description                                        |
| ---------------------------------- | --------- | ----- | -------------------------------------------------- |
| `idx_user_subscribed_events_user`  | user_id   | BTREE | Optimize queries for retrieving user subscriptions |
| `idx_user_subscribed_events_event` | event_id  | BTREE | Optimize queries for event follower lookups        |

---