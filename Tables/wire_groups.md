# Table: `wire_groups`

### **Description**

Represents logical groupings of news wires, allowing for organization, filtering, and categorization of related news sources within the system.

---

## Schema

| Column Name       | Data Type    | Null | Constraints                 | Description                                        |
| ----------------- | ------------ | ---- | --------------------------- | -------------------------------------------------- |
| `id`              | INT4         | NO   | Primary Key, Auto-Increment | Unique identifier for each wire group              |
| `wire_group_name` | VARCHAR(100) | NO   | Unique                      | Name of the group (e.g., `'Top Financial Feeds'`)  |
| `description`     | TEXT         | YES  | –                           | Optional description providing details about group |

---

## Relationships

* Referenced by: `news_wires_2.wire_group_id → wire_groups.id`

---

## Example Record

```json
{
  "id": 2,
  "wire_group_name": "Global Markets Feeds",
  "description": "Includes wires covering international markets and indices"
}
```

---

## Usage Scenarios

* Categorizing related news wires into named groups
* Simplifying filtering or selection in admin dashboards
* Applying group-level policies or visibility settings

---

## Query Examples

### List all wire groups:

```sql
SELECT id, wire_group_name, description
FROM wire_groups
ORDER BY wire_group_name;
```

### Find a group by partial name:

```sql
SELECT *
FROM wire_groups
WHERE wire_group_name ILIKE '%markets%';
```