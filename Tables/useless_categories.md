# Table: `useless_categories`

### **Description**

A legacy or deprecated categorization table, possibly retained for compatibility, archival purposes, or temporary mapping logic. Despite its name, it may still serve hierarchical structuring or internal referencing needs.

---

## Schema

| Column Name         | Data Type    | Null | Constraints                    | Description                                                 |
| ------------------- | ------------ | ---- | ------------------------------ | ----------------------------------------------------------- |
| `id`                | INT4         | NO   | Primary Key, Auto-Increment    | Unique identifier for each (sub)category                    |
| `category_name`     | VARCHAR(100) | NO   | –                              | Name of the main category                                   |
| `subcategory_name`  | VARCHAR(255) | YES  | –                              | Optional subcategory label                                  |
| `subcategory2_name` | VARCHAR(255) | YES  | –                              | Optional secondary subcategory label                        |
| `parent_id`         | INT4         | YES  | Self-referencing FK (optional) | References another row in the same table as parent category |

---

## Hierarchical Relationships

* `parent_id` can reference `id` within the same table to form a **tree structure** of nested categories.

---

## Example Record

```json
{
  "id": 42,
  "category_name": "Commodities",
  "subcategory_name": "Energy",
  "subcategory2_name": "Crude Oil",
  "parent_id": null
}
```

---

## Usage Scenarios

* Mapping outdated or fallback categories during migration
* Representing nested category trees or classification logic
* Supporting legacy filtering logic in archived datasets

---

## Query Examples

### Get all root categories (no parent):

```sql
SELECT * FROM useless_categories
WHERE parent_id IS NULL;
```

### Get all children of a given category:

```sql
SELECT * FROM useless_categories
WHERE parent_id = 42;
```