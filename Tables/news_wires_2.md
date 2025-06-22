# Table: `news_wires_2`

### **Description**

Stores metadata for news wires—distinct news feeds or channels—used for categorizing and ranking news content. Each wire may belong to a group and have a default display rank.

---

## Schema

| Column Name     | Data Type    | Null | Constraints                 | Description                               |
| --------------- | ------------ | ---- | --------------------------- | ----------------------------------------- |
| `id`            | INT4         | NO   | Primary Key, Auto-Increment | Unique identifier for each news wire      |
| `wire_name`     | VARCHAR(100) | NO   | –                           | Name of the news wire (e.g., `'Reuters'`) |
| `description`   | TEXT         | YES  | –                           | Optional description of the news wire     |
| `wire_group_id` | INT4         | YES  | Foreign Key (if applicable) | ID linking the wire to a logical group    |
| `default_rank`  | INT4         | YES  | –                           | Default display or processing rank        |

---

## Example Record

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

* Organizing news articles by their source wire
* Setting display or processing priority using `default_rank`
* Grouping wires logically by `wire_group_id` for bulk operations

---

## Query Examples

### List all wires ordered by default rank:

```sql
SELECT wire_name, default_rank
FROM news_wires_2
ORDER BY default_rank;
```

### Get wire details by name:

```sql
SELECT * 
FROM news_wires_2 
WHERE wire_name ILIKE '%Reuters%';
```