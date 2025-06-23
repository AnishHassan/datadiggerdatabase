# Table: `user_profiles`

---

## Description

Stores additional personal details for each user, enhancing the experience with customized profiles, time zones, and localization settings. Designed to support a global audience, with initial focus on Australia and New Zealand.

---

## Schema

| Column Name           | Data Type    | Null | Default | Constraints     | Description                                             |
| --------------------- | ------------ | ---- | ------- | --------------- | ------------------------------------------------------- |
| `user_id`             | UUID         | NO   | -       | PRIMARY KEY, FK | Unique ID referencing the user (1-to-1 relationship)    |
| `bio`                 | TEXT         | YES  | NULL    |                 | Short biography or "About me" section                   |
| `location`            | VARCHAR(255) | YES  | NULL    |                 | User's city, suburb, or general location                |
| `date_of_birth`       | DATE         | YES  | NULL    |                 | User's date of birth                                    |
| `timezone`            | VARCHAR(50)  | YES  | NULL    |                 | IANA time zone string (e.g., `Australia/Sydney`, `UTC`) |
| `language`            | VARCHAR(50)  | YES  | NULL    |                 | Preferred language code (e.g., `en`, `es`, `zh`)        |
| `profile_picture_url` | TEXT         | YES  | NULL    |                 | URL pointing to user's avatar or hosted profile image   |

---

## Relationships

| Related Table | Relationship Type | Foreign Key | Description                    |
| ------------- | ----------------- | ----------- | ------------------------------ |
| `users`       | One-to-One        | `user_id`   | Each user has a single profile |

---

## Usage Scenarios

1. **User Onboarding**: Automatically create a profile record during user signup.
2. **Profile Personalization**: Display user-specific bio, location, and image in dashboards or forums.
3. **Localization & Scheduling**: Use `timezone` and `language` for formatting dates, times, and translations.
4. **Birthday-Based Features**: Offer birthday perks or reminders using `date_of_birth`.
5. **Avatar Display**: Load `profile_picture_url` across UI components needing profile images.

---

## Query Examples

### 1. Get full profile details for a specific user:

```sql
SELECT *
FROM user_profiles
WHERE user_id = 'a718f920-fb9c-4d3e-8e59-a1e6b78d92c2';
```

### 2. Find all users in New Zealand:

```sql
SELECT user_id
FROM user_profiles
WHERE location ILIKE '%New Zealand%' OR timezone LIKE 'Pacific/Auckland';
```

### 3. Fetch users whose birthday is today:

```sql
SELECT user_id
FROM user_profiles
WHERE EXTRACT(MONTH FROM date_of_birth) = EXTRACT(MONTH FROM CURRENT_DATE)
  AND EXTRACT(DAY FROM date_of_birth) = EXTRACT(DAY FROM CURRENT_DATE);
```

### 4. Get users using non-English languages:

```sql
SELECT user_id, language
FROM user_profiles
WHERE language != 'en';
```

---

## Insert Example

```sql
INSERT INTO user_profiles (
  user_id,
  bio,
  location,
  date_of_birth,
  timezone,
  language,
  profile_picture_url
) VALUES (
  'a718f920-fb9c-4d3e-8e59-a1e6b78d92c2',
  'Outdoor enthusiast and UX designer based in Wellington.',
  'Wellington, NZ',
  '1987-11-22',
  'Pacific/Auckland',
  'en',
  'https://cdn.example.com/profiles/user-a718f920.jpg'
);
```

---