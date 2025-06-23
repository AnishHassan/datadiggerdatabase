# Table: `useless_categories`

### **Description**

A legacy or transitional categorization table that supports hierarchical organization of categories and subcategories. Despite the misleading name, this table may still serve meaningful roles such as fallback classification, backward compatibility, or maintaining referential integrity in migrated systems.

---

## **Schema**

| Column Name         | Data Type    | Null | Constraints                           | Description                                                   |
| ------------------- | ------------ | ---- | ------------------------------------- | ------------------------------------------------------------- |
| `id`                | INT4         | NO   | Primary Key, Auto-Increment           | Unique identifier for each category or subcategory            |
| `category_name`     | VARCHAR(100) | NO   |                                       | The main or top-level category name                           |
| `subcategory_name`  | VARCHAR(255) | YES  |                                       | An optional first-level subcategory associated with the entry |
| `subcategory2_name` | VARCHAR(255) | YES  |                                       | An optional second-level subcategory for finer classification |
| `parent_id`         | INT4         | YES  | Foreign Key → `useless_categories.id` | Optional reference to a parent category in the same table     |

---

## **Relationships**

| Related Table        | Relationship Type | Foreign Key | Description                                      |
| -------------------- | ----------------- | ----------- | ------------------------------------------------ |
| `useless_categories` | Self-referencing  | `parent_id` | Allows nesting of categories in a tree structure |

---

## **Business Rules**

* `parent_id` may be `NULL` for top-level (root) categories.
* Category names must not be empty; they represent a meaningful classification.
* This table may support multi-level nesting up to any practical depth using recursive queries.
* Often used during transitional phases of taxonomy refactoring.

---

## **Indexes**

| Index Name                   | Column      | Type  | Description                                    |
| ---------------------------- | ----------- | ----- | ---------------------------------------------- |
| `useless_categories_pkey`    | `id`        | BTREE | Primary key for unique category identification |
| *(optional for performance)* | `parent_id` | BTREE | Speeds up hierarchy traversal operations       |

---

## **Example Record**

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

## **Usage Scenarios**

* Retaining mappings to legacy categories during data migration.
* Building fallback filters or category selectors for outdated UI features.
* Supporting recursive queries to display multi-tiered classification trees.
* Auditing historical category usage before decommissioning taxonomy structures.

---

## **Query Examples**

### Select all top-level categories:

```sql
SELECT * FROM useless_categories
WHERE parent_id IS NULL;
```

### Get direct children of a category (e.g. `id = 42`):

```sql
SELECT * FROM useless_categories
WHERE parent_id = 42;
```

### Recursive query to build full category tree:

```sql
WITH RECURSIVE category_tree AS (
  SELECT id, category_name, parent_id, 1 AS level
  FROM useless_categories
  WHERE parent_id IS NULL

  UNION ALL

  SELECT uc.id, uc.category_name, uc.parent_id, ct.level + 1
  FROM useless_categories uc
  JOIN category_tree ct ON uc.parent_id = ct.id
)
SELECT * FROM category_tree ORDER BY level, id;
```

---