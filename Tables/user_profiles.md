#  Table: `user_profiles`

# Description
Stores additional personal details for each user, enhancing user experience with customised profiles, time zones, and localisation settings. This table is designed to support global users, with early rollout focused on Australia and New Zealand.

---

#  Schema

| Column Name         | Data Type     | Null | Default | Constraints      | Description                                                  |
|---------------------|---------------|------|---------|------------------|--------------------------------------------------------------|
| user_id             | UUID          | NO   | -       | PRIMARY KEY, FK  | Unique ID referencing the user (1-to-1 relationship)         |
| bio                 | TEXT          | YES  | NULL    |                  | Short biography or user "About me" section                   |
| location            | VARCHAR(255)  | YES  | NULL    |                  | User's city, suburb, or general location                     |
| date_of_birth       | DATE          | YES  | NULL    |                  | User's date of birth                                         |
| timezone            | VARCHAR(50)   | YES  | NULL    |                  | IANA time zone string (e.g., `Australia/Sydney`, `UTC`)      |
| language            | VARCHAR(50)   | YES  | NULL    |                  | Preferred language code (e.g., `en`, `es`, `zh`)             |
| profile_picture_url | TEXT          | YES  | NULL    |                  | URL to the user's avatar or profile photo                   |

---

#  Relationships

| Related Table | Relationship Type | Foreign Key | Description                            |
|---------------|-------------------|-------------|----------------------------------------|
| users         | One-to-One        | user_id     | Each profile corresponds to one user   |

---

#  Business Rules

- A `user_profiles` row is created automatically when a new user is registered.
- Each profile is optional but strongly recommended for personalisation features.
- `timezone` supports platform-wide scheduling and localisation logic.
- `language` enables future internationalisation and multi-language support.
- `profile_picture_url` must point to a valid hosted image file (e.g., CDN).

---

#  Example Row

```json
{
  "user_id": "a718f920-fb9c-4d3e-8e59-a1e6b78d92c2",
  "bio": "Outdoor enthusiast and UX designer based in Wellington.",
  "location": "Wellington, NZ",
  "date_of_birth": "1987-11-22",
  "timezone": "Pacific/Auckland",
  "language": "en",
  "profile_picture_url": "https://cdn.example.com/profiles/user-a718f920.jpg"
}
