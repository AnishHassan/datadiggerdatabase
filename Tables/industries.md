# Table: `industries`

# Description

Stores a list of industry categories that can be used to classify users, companies, charts, or datasets.

---

# Schema

| Column Name | Data Type | Null | Default | Constraints | Description                          |
| ----------- | --------- | ---- | ------- | ----------- | ------------------------------------ |
| id          | INT       | NO   | -       | Primary Key | Unique identifier for the industry   |
| name        | TEXT      | NO   | -       | Unique      | Name of the industry (e.g., Finance) |



# Business Rules

* `name` must be unique and not null.
* Used for categorizing or tagging other entities such as users or datasets.
* Can be used for filtering or analytics by industry type.

---

# Indexes

| Index Name            | Column | Type  | Description                          |
| --------------------- | ------ | ----- | ------------------------------------ |
| `industries_name_key` | name   | BTREE | Ensures uniqueness of industry names |

---

# Example Row

```json
{
  "id": 3,
  "name": "Healthcare"
}
```