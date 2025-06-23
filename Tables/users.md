# Table: `users`

---

## Description

Stores core information about all registered users, including authentication credentials, profile data, login history, and user status flags.
It plays a central role in **authentication**, **authorization**, **user management**, and **personalization** across the platform.

---

## Schema

| Column Name             | Data Type    | Null | Default             | Constraints             | Description                                                  |
| ----------------------  | -----------  | ------------------         | ----------------------- | ------------------------------------------------------------ |
| `id`                    | UUID         | NO   | `gen_random_uuid()` | PRIMARY KEY             | Unique identifier for each user                              |
| `full_name`             | VARCHAR(255) | NO   | -                   |                         | User's full name                                             |
| `email`                 | VARCHAR(255) | NO   | -                   | UNIQUE                  | Email address (used for login and communication)             |
| `password_hash`         | TEXT         | NO   | -                   |                         | Securely hashed password using bcrypt or similar algorithm   |
| `profile_image`         | TEXT         | YES  | NULL                |                         | URL or path to user’s profile picture                        |
| `is_email_verified`     | BOOLEAN      | YES  | `false`             |                         | Indicates whether email has been verified                    |
| `auth_provider`         | VARCHAR(50)  | YES  | `'local'`           |                         | Source of authentication (`local`, `google`, `github`, etc.) |
| `created_at`            | TIMESTAMP    | NO   | `CURRENT_TIMESTAMP` |                         | Timestamp of account creation                                |
| `updated_at`            | TIMESTAMP    | NO   | `CURRENT_TIMESTAMP` |                         | Timestamp of last profile update                             |
| `is_active`             | BOOLEAN      | YES  | `true`              |                         | Whether the account is currently active                      |
| `last_login_at`         | TIMESTAMP    | YES  | NULL                |                         | Timestamp of most recent successful login                    |
| `failed_login_attempts` | INT4         | YES  | `0`                 |                         | Number of consecutive failed login attempts                  |
| `role_id`               | INT4         | YES  | NULL                | FOREIGN KEY → `roles(id)` | Reference to user's role (admin, user,etc.)                |
| `username`              | VARCHAR(100) | YES  | NULL                | UNIQUE                  | Public-facing username, unique per user                      |
| `preferences_id`        | INT4         | YES  | NULL                | FOREIGN KEY → `preferences(id)` | Links to customizable user settings                  |

---

## Relationships

| Related Table            | Type        | Foreign Key      | Description                                              |
| ------------------------ | ----------- | ---------------- | -------------------------------------------------------- |
| `roles`                  | Many-to-One | `role_id`        | Defines the access level or permission group of the user |
| `preferences`            | One-to-One  | `preferences_id` | User's personal preferences (e.g., theme, notifications) |
| `user_sessions`          | One-to-Many | `user_id`        | Tracks login history and active sessions                 |
| `activity_logs`          | One-to-Many | `user_id`        | Records of user interactions across the system           |
| `user_subscribed_events` | One-to-Many | `user_id`        | Tracks which events a user has subscribed to             |

---

## Business Rules

* Email must be unique and is used as the primary login credential.
* Passwords are always hashed using a secure algorithm like **bcrypt** before storage.
* `is_active = false` disables the account without permanent deletion (soft delete).
* `failed_login_attempts` is incremented on failure and reset on successful login.
* `username`, if provided, must be globally unique and is used in public URLs/routes.
* OAuth/social logins (Google, GitHub) populate `auth_provider` and bypass password.
* Deactivation or deletion should cascade or clean up sessions and activity logs.

---

## Example Record

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
```

---

## Usage Scenarios

* User authentication and login via password or OAuth.
* Displaying public profile information (e.g., name, username, profile picture).
* Role-based access control (admin vs user).
* Tracking last login and account health (failed logins).
* Personalizing UI based on user preferences.
* Deactivating or blocking users for violations.

---

## Query Examples

### 1. Get a user by email (login):

```sql
SELECT * FROM users WHERE email = 'anish@example.com';
```

---

### 2. Get active users with verified email:

```sql
SELECT id, full_name, email
FROM users
WHERE is_active = true AND is_email_verified = true;
```

---

### 3. Increment failed login attempts:

```sql
UPDATE users
SET failed_login_attempts = failed_login_attempts + 1
WHERE email = 'anish@example.com';
```

---

### 4. Reset failed attempts & set last login time:

```sql
UPDATE users
SET failed_login_attempts = 0,
    last_login_at = CURRENT_TIMESTAMP
WHERE id = '7f1d5f1a-231d-437c-91a0-3a7de48cb85f';
```

---

### 5. Fetch user with role info:

```sql
SELECT u.id, u.full_name, r.name AS role_name
FROM users u
JOIN roles r ON r.id = u.role_id
WHERE u.id = '7f1d5f1a-231d-437c-91a0-3a7de48cb85f';
```

---

## Insert Example

```sql
INSERT INTO users (
  full_name, email, password_hash, profile_image,
  is_email_verified, auth_provider, username, role_id, preferences_id
) VALUES (
  'Anish Hassan',
  'anish@example.com',
  '$2b$12$6aLxfSAbcdEFGhIJkLMNoOpQrsTUvwxyzABcDEfgh',
  'https://cdn.example.com/profiles/anish.jpg',
  true,
  'local',
  'anishhassan',
  2,
  11
);
```

---

## Suggested Indexes

| Index Name            | Column      | Purpose                            |
| --------------------- | ----------- | ---------------------------------- |
| `idx_users_email`     | `email`     | Fast lookup during login           |
| `idx_users_username`  | `username`  | Support unique user profile URLs   |
| `idx_users_role_id`   | `role_id`   | Optimize role-based queries        |
| `idx_users_is_active` | `is_active` | Quickly find active/inactive users |

---
