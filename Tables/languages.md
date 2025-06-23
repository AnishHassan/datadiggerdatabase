# Table: `languages`

---

## **Description**

Defines supported languages for localization, content tagging, and user interface internationalization. Each language is identified by a two-letter [ISO 639-1](https://en.wikipedia.org/wiki/List_of_ISO_639-1_codes) code, ensuring global compatibility and standardized language handling.

---

## **Schema**

| Column Name     | Data Type    | Null | Default | Constraints                 | Description                                      |
| --------------- | ------------ | ---- | ------- | --------------------------- | ------------------------------------------------ |
| `id`            | INT4         | NO   | –       | Primary Key, Auto-Increment | System-assigned unique ID for each language      |
| `language_code` | BPCHAR(2)    | NO   | –       | Unique                      | ISO 639-1 two-letter code (e.g., `'en'`, `'fr'`) |
| `language_name` | VARCHAR(100) | NO   | –       |                             | Full English name of the language                |

---

## **Business Rules**

* `language_code` must be unique and conform to ISO 639-1 standards.
* `language_name` should represent the full English display name of the language.
* Typically consumed by front-end UI systems, translation layers, or content delivery mechanisms.

---

## **Indexes**

| Index Name                    | Column         | Type  | Description                                |
| ----------------------------- | -------------- | ----- | ------------------------------------------ |
| `languages_pkey`              | id             | BTREE | Primary key for language identity          |
| `languages_language_code_key` | language_code  | BTREE | Enforces uniqueness for ISO language codes |

---

## **Example Record**

```json
{
  "id": 3,
  "language_code": "de",
  "language_name": "German"
}
```

---

## **Usage Scenarios**

* Rendering localized UI elements or documentation based on user preferences.
* Tagging datasets or articles with language metadata.
* Supporting multilingual data pipelines or APIs.

---

## **Query Examples**

### List all supported languages alphabetically:

```sql
SELECT language_code, language_name
FROM languages
ORDER BY language_name;
```

---

### Fetch a specific language by code:

```sql
SELECT * 
FROM languages 
WHERE language_code = 'es';
```

---

### Count of supported languages:

```sql
SELECT COUNT(*) FROM languages;
```