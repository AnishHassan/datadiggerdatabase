# Table: `roles`

---

## Description

The `roles` table defines the **distinct access control roles** within the system. Roles govern permission boundaries, enabling **role-based access control (RBAC)** for different types of users (e.g., `admin`, `moderator`, `user`, `guest`).

---

## Schema

| Column Name | Data Type | Null | Default | Constraints | Description                                          |
| ----------- | --------- | ---- | ------- | ----------- | ---------------------------------------------------- |
| `id`        | INT       | NO   | —       | Primary Key | Unique identifier for each role                      |
| `name`      | VARCHAR   | NO   | —       | Unique      | Descriptive name of the role (e.g., `admin`, `user`) |

---

## Relationships

| Related Table | Relationship Type | Foreign Key | Description                                     |
| ------------- | ----------------- | ----------- | ----------------------------------------------- |
| `user_roles`  | One-to-Many       | `id`        | A single role can be assigned to multiple users |

> Enables **many-to-many** user-role mapping through the `user_roles` join table.

---

## Constraints & Indexes

| Index Name       | Column | Type   | Description                         |
| ---------------- | ------ | ------ | ----------------------------------- |
| `roles_name_key` | `name` | UNIQUE | Prevents duplicate role definitions |

---

## Business Rules

* Role `name` values must be **unique**, human-readable, and preferably in lowercase or kebab-case.
* Role names should reflect clear responsibilities, e.g., `admin`, `moderator`, `editor`, `viewer`.
* Can be extended to integrate with **permission tables** for fine-grained access control.
* Typical usage involves assigning roles during **user registration**, **admin management**, or **feature gating**.

---

## Example Record

```json
{
  "id": 1,
  "name": "admin"
}
```

---

## Query Examples

### List all available roles:

```sql
SELECT * FROM roles ORDER BY name;
```

### Get role by name:

```sql
SELECT id FROM roles WHERE name = 'moderator';
```

### Get all users with the role `admin` (via `user_roles` join):

```sql
SELECT u.*
FROM users u
JOIN user_roles ur ON u.id = ur.user_id
JOIN roles r ON ur.role_id = r.id
WHERE r.name = 'admin';
```

---