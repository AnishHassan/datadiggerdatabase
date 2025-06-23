# Table: `news_wires_2`

---

## Description

Represents metadata for **news wires**—distinct news sources or content feeds—used to classify, group, and prioritize real-time news entries. Each wire can optionally belong to a group (`wire_group_id`) and be assigned a `default_rank` to determine display or processing order.

---

## Schema

| Column Name     | Data Type    | Null | Constraints                 | Description                                                  |
| --------------- | ------------ | ---- | --------------------------- | ------------------------------------------------------------ |
| `id`            | INT          | NO   | Primary Key, Auto-Increment | Unique identifier for each news wire                         |
| `wire_name`     | VARCHAR(100) | NO   | –                           | Human-readable name of the wire (e.g., `'Reuters'`, `'ECB'`) |
| `description`   | TEXT         | YES  | –                           | Optional textual description of the news wire                |
| `wire_group_id` | INT          | YES  | Foreign Key (optional)      | Logical grouping ID for bulk operations or categorization    |
| `default_rank`  | INT          | YES  | –                           | Default display or processing priority                       |

---

## Relationships

| Referenced Table         | Foreign Key     | Type        | Description                                             |
| ------------------------ | --------------- | ----------- | ------------------------------------------------------- |
| `wire_groups` (optional) | `wire_group_id` | Many-to-One | Optional relationship to a table grouping similar wires |

> *Note: If `wire_group_id` is used, consider introducing a supporting `wire_groups` table with descriptive group metadata.*

---

##  Business Rules

* `wire_name` must be unique and descriptive enough to distinguish the news wire.
* `default_rank` can be used to determine importance or rendering order in UI/UX.
* If a `wire_group_id` is present, it should correspond to a valid group.

---

##  Example Record

```json
{
  "id": 5,
  "wire_name": "Bloomberg",
  "description": "Business and financial news feed",
  "wire_group_id": 2,
  "default_rank": 1
}
```

---

## Usage Scenarios

* **Categorization**: Organize news content by their originating wire (e.g., Bloomberg, Reuters).
* **Display Ranking**: Prioritize certain wires on dashboards using `default_rank`.
* **Grouping**: Apply filters or features to wires in the same `wire_group_id`.

---

## Query Examples

### List All Wires by Priority

```sql
SELECT wire_name, default_rank
FROM news_wires_2
ORDER BY default_rank;
```

### Fetch Wire Metadata by Name

```sql
SELECT * 
FROM news_wires_2 
WHERE wire_name ILIKE '%Reuters%';
```

### Find Wires in a Specific Group

```sql
SELECT *
FROM news_wires_2
WHERE wire_group_id = 2;
```