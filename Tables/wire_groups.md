# Table: `wire_groups`

### **Description**

The `wire_groups` table defines logical groupings of news wires. These groups enable better organization, filtering, and classification of news sources across the platform, especially in admin interfaces and user-facing filters.

---

## **Schema**

| Column Name       | Data Type    | Nullable | Constraints                 | Description                                                           |
| ----------------- | ------------ | -------- | --------------------------- | --------------------------------------------------------------------- |
| `id`              | INT4         | NO       | Primary Key, Auto-Increment | Unique identifier for each wire group                                 |
| `wire_group_name` | VARCHAR(100) | NO       | UNIQUE                      | Descriptive, unique name of the group (e.g., `"Top Financial Feeds"`) |
| `description`     | TEXT         | YES      | –                           | Optional detailed description of the wire group’s purpose             |

---

## **Relationships**

| Related Table  | Relationship Type | Foreign Key          | Description                                             |
| -------------- | ----------------- | -------------------- | ------------------------------------------------------- |
| `news_wires_2` | Many-to-One       | `wire_group_id → id` | Each news wire belongs to one group via `wire_group_id` |

---

## **Business Use Cases**

* **Categorization**: Organize similar news wires under thematic groups (e.g., Finance, Politics, Global Markets).
* **Filtering**: Power admin UI filters or user-facing options to view specific types of content.
* **Group-level Policies**: Apply visibility rules or configurations at the group level instead of wire-by-wire.

---

## **Example Record**

```json
{
  "id": 2,
  "wire_group_name": "Global Markets Feeds",
  "description": "Includes wires covering international markets and indices"
}
```

---

## **Sample Queries**

### 1. List all wire groups alphabetically:

```sql
SELECT id, wire_group_name, description
FROM wire_groups
ORDER BY wire_group_name;
```

### 2. Search for wire groups containing a keyword:

```sql
SELECT *
FROM wire_groups
WHERE wire_group_name ILIKE '%markets%';
```