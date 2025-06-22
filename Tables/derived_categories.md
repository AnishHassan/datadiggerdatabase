# Table: `derived_categories`

### **Description**

Stores derived or secondary categories linked to a main category. Useful for building hierarchical or enriched category structures from base category definitions.

---

## Schema

| Column Name             | Data Type    | Null | Constraints                                         | Description                                      |
| ----------------------- | ------------ | ---- | --------------------------------------------------- | ------------------------------------------------ |
| `id`                    | INT4         | NO   | Primary Key, Auto-Increment                         | Unique identifier for the derived category       |
| `main_category_id`      | INT4         | YES  | Indexed (`idx_derived_categories_main_category_id`) | Reference to the main category this derives from |
| `derived_category_name` | VARCHAR(255) | YES  | -                                                   | Name of the derived category                     |

---

## Indexes

| Index Name                                | Columns            | Type  | Description                         |
| ----------------------------------------- | ------------------ | ----- | ----------------------------------- |
| `idx_derived_categories_main_category_id` | `main_category_id` | BTREE | Optimizes lookup by parent category |

---

## Relationships

* `main_category_id` is expected to reference a category in another table (e.g., `calendar_categories.id` or similar).

---

## Example Record

```json
{
  "id": 101,
  "main_category_id": 5,
  "derived_category_name": "Retail Inflation"
}
```

---

## Usage Scenarios

* Building dynamic category hierarchies for filtering, navigation, or reporting.
* Enriching or grouping calendar event data under more descriptive tags.
* Supporting alternative naming/taxonomy schemes for main categories.

---

## Query Examples

### Get all derived categories for a given main category:

```sql
SELECT derived_category_name
FROM derived_categories
WHERE main_category_id = 5;
```

### Count how many derived categories exist per main category:

```sql
SELECT main_category_id, COUNT(*) AS total_derived
FROM derived_categories
GROUP BY main_category_id
ORDER BY total_derived DESC;
```