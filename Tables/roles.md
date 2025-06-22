# Table: `roles`

# Description

Defines distinct roles available in the system (e.g., Admin, Moderator, User). Helps manage access control and permission levels.

---

# Schema

| Column Name | Data Type | Null | Default | Constraints | Description                              |
| ----------- | --------- | ---- | ------- | ----------- | ---------------------------------------- |
| id          | INT       | NO   | -       | Primary Key | Unique identifier for each role          |
| name        | VARCHAR   | NO   | -       | Unique      | Name of the role (e.g., `admin`, `user`) |

---

# Relationships

| Related Table | Relationship Type | Foreign Key | Description                          |
| ------------- | ----------------- | ----------- | ------------------------------------ |
| user_roles    | One-to-Many       | id          | A role can be assigned to many users |

---

# Business Rules

* Role names should be unique and meaningful (e.g., lowercase or kebab-case).
* Common roles may include `admin`, `moderator`, `member`, `guest`.
* Use this table in conjunction with a join table (`user_roles`) for flexibility in assigning multiple roles per user (if applicable).

---

# Indexes

| Index Name       | Column | Type   | Description                       |
| ---------------- | ------ | ------ | --------------------------------- |
| `roles_name_key` | name   | UNIQUE | Enforces uniqueness of role names |

---

# Example Row

```json
{
  "id": 1,
  "name": "admin"
}
```