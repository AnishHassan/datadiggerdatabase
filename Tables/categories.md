# Table: `categories`

---

# Description

Represents a hierarchical classification structure for organizing economic or market-related calendar events. Each category can include up to two levels of subcategories and supports nested relationships via self-referencing `parent_id`.

---

## Schema

| Column Name        | Data Type    | Null | Default            | Constraints | Description                                              |
| ------------------ | ------------ | ---- | ------------------ | ----------- | -------------------------------------------------------- |
| id                 | INT          | NO   | -                  | Primary Key | Unique identifier for each category                      |
| category_name      | VARCHAR(255) | NO   | -                  |             | Name of the main category                                |
| subcategory_name   | VARCHAR(255) | YES  | NULL               |             | Optional first-level subcategory                         |
| subcategory2_name  | VARCHAR(255) | YES  | NULL               |             | Optional second-level subcategory                        |
| parent_id          | INT          | YES  | NULL               |             | Self-referential key to another category (nested levels) |
| created_at         | TIMESTAMPTZ  | NO   | CURRENT_TIMESTAMP  |             | Timestamp when the category was created                  |
| updated_at         | TIMESTAMPTZ  | NO   | CURRENT_TIMESTAMP  |             | Timestamp when the category was last updated             |

---

## Relationships

| Related Table    | Relationship Type | Foreign Key  | Description                                     |
| ---------------- | ----------------- | ------------ | ----------------------------------------------- |
| categories       | Self-Referential  | parent_id    | Allows nested category structure (parent-child) |
| calendar_events  | One-to-Many       | category_id  | Links category to one or more calendar events   |

---

## Example Record

```json
{
  "id": 10,
  "category_name": "Technology",
  "subcategory_name": "Software",
  "subcategory2_name": "AI",
  "parent_id": null,
  "created_at": "2025-06-21T12:00:00Z",
  "updated_at": "2025-06-21T12:00:00Z"
}
```

---

## Usage Scenarios

* Organize economic events into meaningful hierarchical structures for filtering and analysis.
* Group calendar events by sectors such as `Technology`, `Finance`, or `Energy`.
* Display a nested tree of categories in dashboards, reports, or analytics tools.
* Enable advanced drill-down navigation across multi-level categories and subcategories.

---

## Query Examples

### 1. Get all categories under the main category “Technology”:

```sql
SELECT *
FROM categories
WHERE category_name = 'Technology';
```

### 2. Fetch all subcategories under “Finance”:

```sql
SELECT *
FROM categories
WHERE category_name = 'Finance' AND subcategory_name IS NOT NULL;
```

### 3. Find full category path for a given `category_id`:

```sql
SELECT 
  id,
  category_name || 
  COALESCE(' > ' || subcategory_name, '') || 
  COALESCE(' > ' || subcategory2_name, '') AS full_path
FROM categories
WHERE id = 10;
```

### 4. Get all categories nested under a specific `parent_id`:

```sql
SELECT *
FROM categories
WHERE parent_id = 5;
```

---

## Insert Example

```sql
INSERT INTO categories (
  id, category_name, subcategory_name, subcategory2_name, parent_id
)
VALUES (
  11, 'Finance', 'Banking', 'Retail', NULL
);
```