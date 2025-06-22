# Table: `languages`

### **Description**

Stores supported languages for internationalization/localization, including language codes and their full names. Useful for multilingual interfaces, data tagging, or country-specific content delivery.

---

## Schema

| Column Name     | Data Type    | Null | Constraints                 | Description                                    |
| --------------- | ------------ | ---- | --------------------------- | ---------------------------------------------- |
| `id`            | INT4         | NO   | Primary Key, Auto-Increment | Unique identifier for each language            |
| `language_code` | BPCHAR(2)    | NO   | Unique                      | ISO 639-1 language code (e.g., `'en'`, `'fr'`) |
| `language_name` | VARCHAR(100) | NO   | –                           | Full name of the language (e.g., `'English'`)  |

---

## Example Record

```json
{
  "id": 3,
  "language_code": "de",            `                                                                           
  "language_name": "German"
}
```

---

## Usage Scenarios

* Displaying UI in multiple languages
* Associating content or metadata with specific languages
* Supporting localization efforts across countries and regions

---

## Query Examples

### Get all supported languages:

```sql
SELECT language_code, language_name
FROM languages
ORDER BY language_name;
```

### Find language by code:

```sql
SELECT * 
FROM languages 
WHERE language_code = 'es';
```