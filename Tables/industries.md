# Table: `industries`

---

## **Description**

Stores a curated list of industry categories for classifying and tagging other entities such as **users**, **companies**, **datasets**, or **charts**. Enables filtering, analytics, and segmentation by industry.

---

## **Schema**

| Column Name | Data Type | Null | Default | Constraints | Description                                                |
| ----------- | --------- | ---- | ------- | ----------- | ---------------------------------------------------------- |
| `id`        | INT       | NO   | –       | Primary Key | Unique numeric identifier for the industry                 |
| `name`      | TEXT      | NO   | –       | Unique      | Canonical name of the industry (e.g., "Finance", "Energy") |

---

## **Business Rules**

* `name` **must be unique** and non-null to prevent duplicates or ambiguous categorizations.
* Used as a **taxonomy layer** for classification, filtering, or analytics across systems.
* Ideal for dropdowns, filters, or joins with other metadata tables (e.g., `users.industry_id`, `datasets.industry_id`).

---

## **Indexes**

| Index Name            | Column | Type  | Description                           |
| --------------------- | ------ | ----- | ------------------------------------- |
| `industries_name_key` | name   | BTREE | Enforces uniqueness on industry names |

---

## **Example Record**

```json
{
  "id": 3,
  "name": "Healthcare"
}
```

---

## **Usage Scenarios**

* Tagging users, data sources, or companies with their relevant industry.
* Powering industry-specific dashboards or filters.
* Supporting analytics like “Top 5 datasets by industry.”

---

## **Query Examples**

### Get all industries sorted alphabetically:

```sql
SELECT * FROM industries
ORDER BY name;
```

---

### Find industry by name (case-insensitive):

```sql
SELECT * FROM industries
WHERE LOWER(name) = LOWER('Finance');
```

---

### Count how many datasets are associated with each industry (if used with a `datasets` table):

```sql
SELECT i.name, COUNT(d.id) AS dataset_count
FROM industries i
JOIN datasets d ON d.industry_id = i.id
GROUP BY i.name
ORDER BY dataset_count DESC;
```