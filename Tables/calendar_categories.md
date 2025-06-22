# Table: `calendar_categories`

# **Description**

Defines a hierarchical system of categories for organizing economic calendar events or indicators. Supports parent-child nesting and display ordering for structured classification and UI presentation.

---

# Schema

| Column Name     | Data Type    | Null | Constraints                 | Description                                    |
| --------------- | ------------ | ---- | --------------------------- | ---------------------------------------------- |
| `id`            | INT4         | NO   | Primary Key, Auto-Increment | Unique identifier for each category            |
| `name`          | VARCHAR(100) | NO   | Unique                      | Name of the category                           |
| `parent_id`     | INT4         | YES  | –                           | References the parent category (for hierarchy) |
| `display_order` | INT4         | YES  | Default: `0`                | Order for displaying categories in the UI      |

---

# Indexes

| Index Name                          | Columns     | Type  | Description                             |
| ----------------------------------- | ----------- | ----- | --------------------------------------- |
| `idx_calendar_categories_parent_id` | `parent_id` | BTREE | Optimizes retrieval of child categories |

---

# Relationships

* `parent_id` → `calendar_categories.id`: Self-referential foreign key supporting hierarchical structure (e.g., subcategories under "Economy").

---

# Usage Scenarios

* Grouping economic indicators into major themes (e.g., "Inflation", "Employment").
* Generating navigable category trees for UI filters or menus.
* Controlling the display priority/order of categories.

---

# Example Record

```json
{
  "id": 12,
  "name": "Inflation",
  "parent_id": 3,
  "display_order": 2
}
```

---

# Query Examples

# Get all top-level categories:

```sql
SELECT id, name
FROM calendar_categories
WHERE parent_id IS NULL
ORDER BY display_order;
```

---

# Get all subcategories under a parent:

```sql
SELECT id, name
FROM calendar_categories
WHERE parent_id = 12
ORDER BY display_order;
```

---

# Insert Example

```sql
INSERT INTO calendar_categories (name, parent_id, display_order)
VALUES ('Employment', NULL, 1);
```