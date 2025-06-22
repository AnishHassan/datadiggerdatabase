# Table: `users`

# Description
Stores core information about platform users, including authentication credentials, status flags, user preferences, and login metadata. This table is fundamental to authentication, user management, and personalization across the system.

---

# Schema

| Column Name          | Data Type      | Null | Default              | Constraints         | Description                                                 |
|----------------------|----------------|------|-----------------------|----------------------|-------------------------------------------------------------|
| id                   | UUID           | NO   | `gen_random_uuid()`   | PRIMARY KEY          | Unique identifier for each user                             |
| full_name            | VARCHAR(255)   | NO   | -                     |                      | Full name of the user                                       |
| email                | VARCHAR(255)   | NO   | -                     | UNIQUE               | User's email address, must be unique                        |
| password_hash        | TEXT           | NO   | -                     |                      | Encrypted password hash                                     |
| profile_image        | TEXT           | YES  | NULL                  |                      | URL/path to profile image                                   |
| is_email_verified    | BOOLEAN        | YES  | `false`               |                      | Indicates if email is verified                              |
| auth_provider        | VARCHAR(50)    | YES  | `'local'`             |                      | Authentication source (e.g. `local`, `google`, `github`)    |
| created_at           | TIMESTAMP      | NO   | `CURRENT_TIMESTAMP`   |                      | Timestamp of account creation                               |
| updated_at           | TIMESTAMP      | NO   | `CURRENT_TIMESTAMP`   |                      | Last profile update timestamp                               |
| is_active            | BOOLEAN        | YES  | `true`                |                      | Whether the account is active                               |
| last_login_at        | TIMESTAMP      | YES  | NULL                  |                      | Last successful login time                                  |
| failed_login_attempts| INT4           | YES  | `0`                   |                      | Count of consecutive failed login attempts                  |
| role_id              | INT4           | YES  | NULL                  | FOREIGN KEY (roles)  | User's role (admin, user, etc.)                             |
| username             | VARCHAR(100)   | YES  | NULL                  | UNIQUE               | Unique public username                                      |
| preferences_id       | INT4           | YES  | NULL                  | FOREIGN KEY          | ID linking to user preference settings                      |

---

#  Relationships

| Related Table     | Relationship Type | Foreign Key         | Description                                           |
|-------------------|-------------------|----------------------|-------------------------------------------------------|
| roles             | Many-to-One       | `role_id`            | Defines user access level or permissions              |
| preferences       | One-to-One        | `preferences_id`     | Contains user-specific settings and preferences       |
| user_sessions     | One-to-Many       | `user_id`            | Tracks user login sessions                            |
| activity_logs     | One-to-Many       | `user_id`            | Logs of user actions across the system                |

---

#  Business Rules

- Email must be unique and validated upon registration.
- Passwords must be hashed using a secure algorithm (e.g., bcrypt).
- `failed_login_attempts` is incremented after each failed login attempt and reset on success.
- Inactive users (`is_active = false`) are not allowed to log in.
- `username` is used for display or public routes and must be unique.
- Soft deletes can be implemented by toggling `is_active`.

---

# Example Row

```json
{
  "id": "7f1d5f1a-231d-437c-91a0-3a7de48cb85f",
  "full_name": "Anish Hassan",
  "email": "anish@example.com",
  "password_hash": "$2b$12$6aLxfSAbcdEFGhIJkLMNoOpQrsTUvwxyzABcDEfgh",
  "profile_image": "https://cdn.example.com/profiles/anish.jpg",
  "is_email_verified": true,
  "auth_provider": "local",
  "created_at": "2025-01-01T10:00:00Z",
  "updated_at": "2025-06-20T16:45:00Z",
  "is_active": true,
  "last_login_at": "2025-06-20T16:00:00Z",
  "failed_login_attempts": 0,
  "role_id": 2,
  "username": "anishhassan",
  "preferences_id": 11
}
