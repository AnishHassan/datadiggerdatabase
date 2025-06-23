# Table: `user_activity_feed`

### **Description**

Captures a unified feed of user-initiated and system-triggered events to support:

* **Auditing and compliance**
* **User notification systems**
* **Engagement analytics**
* **Activity-based triggers and workflows**

Each entry reflects a discrete action related to a user or their associated resources.

---

## **Schema**

| Column Name     | Data Type    | Null | Default             | Constraints                 | Description                                                   |
| --------------- | ------------ | ---- | ------------------- | --------------------------- | ------------------------------------------------------------- |
| `id`            | INT          | NO   | -                   | Primary Key, Auto-increment | Unique identifier for each activity                           |
| `user_id`       | UUID         | YES  | -                   | Foreign Key → `users(id)`   | The user responsible for or related to the activity           |
| `activity_type` | VARCHAR(100) | YES  | -                   |                             | Categorical type of activity                                  |
| `reference_id`  | UUID         | YES  | -                   |                             | Optional reference to related object (dataset, chart, etc.)   |
| `message`       | TEXT         | YES  | -                   |                             | Readable description of the activity                          |
| `status`        | VARCHAR(30)  | YES  | -                   |                             | Status of the activity (e.g., `success`, `failed`, `pending`) |
| `created_at`    | TIMESTAMP    | NO   | `CURRENT_TIMESTAMP` |                             | Timestamp when the activity occurred                          |

---

## **Relationships**

| Related Table | Relationship Type | Foreign Key | Description                                    |
| ------------- | ----------------- | ----------- | ---------------------------------------------- |
| `users`       | Many-to-One       | `user_id`   | Associates the activity with a registered user |

---

## **Business Rules**

* Activities may be user-triggered or system-generated (e.g., auto-renewal, email notification sent).
* `message` must be written in a user-friendly and **i18n-compatible** (internationalized) format.
* `reference_id` should be used to associate the activity with another record, such as:

  * A dataset (`dataset_uploaded`)
  * A chart (`chart_saved`)
  * An event or subscription (`event_subscribed`)
* Indexing on `created_at`, `user_id`, and `activity_type` is recommended for query performance.

---

## **Common Values: `activity_type`**

| Type                   | Description                               |
| ---------------------- | ----------------------------------------- |
| `login`                | User logged in                            |
| `logout`               | User logged out                           |
| `password_changed`     | Password was successfully updated         |
| `dataset_uploaded`     | Dataset was uploaded by the user          |
| `chart_saved`          | Chart was created or updated              |
| `subscription_updated` | Subscription plan was changed or renewed  |
| `event_subscribed`     | User subscribed to an event               |
| `account_deleted`      | User deleted or deactivated their account |

---

## **Example Record**

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
```

---

## **Indexing Recommendations**

| Index Name                       | Column(s)         | Purpose                                 |
| -------------------------------- | ----------------- | --------------------------------------- |
| `user_activity_feed_user_idx`    | `user_id`         | Quickly retrieve a user's activity feed |
| `user_activity_feed_type_idx`    | `activity_type`   | Support filtering by event types        |
| `user_activity_feed_created_idx` | `created_at DESC` | Optimize for most recent activity       |

---

## **Query Examples**

### Get the latest 10 activities for a user:

```sql
SELECT * FROM user_activity_feed
WHERE user_id = 'bfe2e0e7-1234-4d9c-881a-782e0f72ae7a'
ORDER BY created_at DESC
LIMIT 10;
```

### Get all failed system activities:

```sql
SELECT * FROM user_activity_feed
WHERE status = 'failed'
AND user_id IS NULL;
```

### Trace a specific dataset's activity log:

```sql
SELECT * FROM user_activity_feed
WHERE reference_id = 'a3e91234-cb56-44ab-bf79-d891b3af9812'
ORDER BY created_at;
```

---

## **Enhancement Ideas (Optional)**

* Add `source` column (`'web'`, `'mobile'`, `'api'`) to trace activity origin.
* Introduce `metadata` (JSONB) for extensible logging (e.g., browser agent, IP address).
* Enable real-time notifications via a trigger mechanism or pub/sub queue.
* Add `is_read` boolean for notification-based consumption in UI.

---