# Table: `derived_categories`

---

# Description

Stores secondary or derived categories that branch from a primary category. This structure supports enriched hierarchies, alternative naming schemes, and flexible tagging for classification systems such as event calendars or dashboards.

---

# Schema

| Column Name             | Data Type    | Null | Default | Constraints                                         | Description                                |
| ----------------------- | ------------ | ---- | ------- | --------------------------------------------------- | ------------------------------------------ |
| `id`                    | INT4         | NO   | -       | Primary Key, Auto-Increment                         | Unique identifier for the derived category |
| `main_category_id`      | INT4         | YES  | NULL    | Indexed (`idx_derived_categories_main_category_id`) | References the parent or base category     |
| `derived_category_name` | VARCHAR(255) | YES  | NULL    |                                                     | Name of the derived or sub-category        |

---

# Indexes

| Index Name                                | Columns            | Type  | Description                                 |
| ----------------------------------------- | ------------------ | ----- | ------------------------------------------- |
| `idx_derived_categories_main_category_id` | `main_category_id` | BTREE | Optimizes queries filtered by main category |

---

# Relationships

| Related Table                  | Relationship Type | Foreign Key        | Description                                                 |
| ------------------------------ | ----------------- | ------------------ | ----------------------------------------------------------- |
| *(e.g. `calendar_categories`)* | Many-to-One       | `main_category_id` | Each derived category links to a main category (logical FK) |

> **Note:** The foreign key is expected but not explicitly enforced. It assumes a logical connection to a main category table such as `calendar_categories`.

---

# Example Record

```json
{
  "id": 101,
  "main_category_id": 5,
  "derived_category_name": "Retail Inflation"
}
```

---

# Usage Scenarios

* Generating nested filters for dashboards, charts, or navigation menus.
* Organizing complex datasets using layered category structures.
* Supporting multilingual or region-specific naming for base categories.
* Building taxonomy or tagging systems that evolve from base categories.

---

# Query Examples

# Get all derived categories for a specific main category:

```sql
SELECT derived_category_name
FROM derived_categories
WHERE main_category_id = 5;
```

# Count total derived categories per main category:

```sql
SELECT main_category_id, COUNT(*) AS total_derived
FROM derived_categories
GROUP BY main_category_id
ORDER BY total_derived DESC;
```

# Search derived categories by partial name:

```sql
SELECT *
FROM derived_categories
WHERE derived_category_name ILIKE '%inflation%';
```

---

# Insert Example

```sql
INSERT INTO derived_categories (
  main_category_id,
  derived_category_name
) VALUES (
  5,
  'Retail Inflation'
);
```