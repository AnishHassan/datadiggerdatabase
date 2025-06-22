# Table: `chart_datasets`

---

# Description

Defines the relationship between charts and datasets by linking them together. This table enables charts to reference one or more datasets, with each relationship annotated by type (e.g., `primary`, `supporting`). Useful for visualizations that draw data from multiple sources.

---

# Schema

| Column Name        | Data Type | Null | Default             | Constraints | Description                                                  |
| ------------------ | --------- | ---- | ------------------- | ----------- | ------------------------------------------------------------ |
| id                 | UUID      | NO   | `gen_random_uuid()` | Primary Key | Unique identifier for the chart-dataset relationship         |
| chart_id           | UUID      | YES  | NULL                | Foreign Key | References the chart using the dataset                       |
| dataset_id         | UUID      | YES  | NULL                | Foreign Key | References the dataset being used                            |
| relationship_type  | TEXT      | YES  | `'primary'`         |             | Role of the dataset in the chart (e.g., primary, supporting) |
| created_at         | TIMESTAMP | NO   | `CURRENT_TIMESTAMP` |             | Timestamp when the relationship was created                  |

---

# Relationships

| Related Table | Relationship Type | Foreign Key | Description                          |
| ------------- | ----------------- | ----------- | ------------------------------------ |
| charts        | Many-to-One       | chart_id    | A chart can reference many datasets  |
| datasets      | Many-to-One       | dataset_id  | A dataset can be used in many charts |

---

## Example Record

```json
{
  "id": "ff318f7e-212c-4ef5-bdc3-587ba2fd64c5",
  "chart_id": "cb0cf0aa-77d2-4410-8ed4-b4720f76a94b",
  "dataset_id": "f12c9c87-8c2b-4d47-942b-23f8d8baf7d3",
  "relationship_type": "primary",
  "created_at": "2025-06-21T15:15:00Z"
}
```

---

## Usage Scenarios

* Display all datasets used in a specific chart (e.g., main vs. reference data).
* Track when a dataset was added to a chart configuration.
* Build layered or comparative charts by tagging datasets as `primary` or `supporting`.
* Enable flexible chart composition via dynamic data bindings.

---

# Query Examples

# 1. Retrieve all datasets used in a specific chart:

```sql
SELECT dataset_id, relationship_type
FROM chart_datasets
WHERE chart_id = 'cb0cf0aa-77d2-4410-8ed4-b4720f76a94b';
```

# 2. List all charts that use a given dataset:

```sql
SELECT chart_id
FROM chart_datasets
WHERE dataset_id = 'f12c9c87-8c2b-4d47-942b-23f8d8baf7d3';
```

# 3. Get charts with primary datasets only:

```sql
SELECT chart_id, dataset_id
FROM chart_datasets
WHERE relationship_type = 'primary';
```

---

# Insert Example

```sql
INSERT INTO chart_datasets (
  id, chart_id, dataset_id, relationship_type
)
VALUES (
  gen_random_uuid(),
  'cb0cf0aa-77d2-4410-8ed4-b4720f76a94b',
  'f12c9c87-8c2b-4d47-942b-23f8d8baf7d3',
  'primary'
);
```