# Database Exploration Notes – Sakila

## 1. Database Information

| Field         | Details                 |
| ------------- | ----------------------- |
| Project       | Sakila Database Testing |
| Database      | Sakila                  |
| Database Type | Relational Database     |
| DBMS          | MySQL                   |
| Tool          | MySQL Workbench         |
| Tester        | Shreya Mandrekar        |
| Testing Type  | Database Testing        |

---

## 2. Exploration Objective

The objective of this exploration is to understand the actual Sakila database structure, identify tables and foreign-key relationships, review important table schemas, and determine suitable areas for database testing.

The exploration was performed using MySQL Workbench against the local Sakila database.

---

## 3. Database Verification

The active database was verified using:

```sql
SELECT DATABASE();
```

**Observed Database:** `sakila`

The database contains 23 database objects based on the `SHOW FULL TABLES FROM sakila` query, consisting of 16 base tables and 7 views.

---

## 4. Database Objects Identified

The Sakila database contains the following database objects:

* `actor`
* `actor_info`
* `address`
* `category`
* `city`
* `country`
* `customer`
* `customer_list`
* `film`
* `film_actor`
* `film_category`
* `film_text`
* `inventory`
* `language`
* `payment`
* `rental`
* `staff`
* `store`
* `film_list`
* `nicer_but_slower_film_list`
* `sales_by_store`
* `sales_by_film_category`
* `sales_by_staff`
* `staff_list`

The database includes both base tables and database views.

---

## 5. Customer Table Exploration

The `customer` table was reviewed using:

```sql
DESCRIBE customer;
```

The table contains 9 columns:

| Column        | Data Type           | NULL | Key | Extra          |
| ------------- | ------------------- | ---- | --- | -------------- |
| `customer_id` | `smallint unsigned` | NO   | PRI | auto_increment |
| `store_id`    | `tinyint unsigned`  | NO   | MUL |                |
| `first_name`  | `varchar(45)`       | NO   |     |                |
| `last_name`   | `varchar(45)`       | NO   | MUL |                |
| `email`       | `varchar(50)`       | YES  |     |                |
| `address_id`  | `smallint unsigned` | NO   | MUL |                |
| `active`      | `tinyint(1)`        | NO   |     |                |
| `create_date` | `datetime`          | NO   |     |                |
| `last_update` | `timestamp`         | YES  |     |                |

### Initial Observations

* `customer_id` is the primary key.
* `customer_id` is auto-incremented.
* `store_id` and `address_id` are indexed columns.
* `first_name` and `last_name` are mandatory fields.
* `email` allows `NULL` values.
* `address_id` is mandatory.
* `active` is a mandatory field.
* `create_date` is mandatory.
* `last_update` allows `NULL` values.

These observations provide potential areas for data integrity and validation testing.

---

## 6. Foreign-Key Relationships

Foreign-key relationships were explored using `INFORMATION_SCHEMA.KEY_COLUMN_USAGE`.

The foreign-key exploration query returned 22 foreign-key relationships across the Sakila database.

Important relationships identified include:

| Table           | Column                 | Referenced Table | Referenced Column |
| --------------- | ---------------------- | ---------------- | ----------------- |
| `address`       | `city_id`              | `city`           | `city_id`         |
| `city`          | `country_id`           | `country`        | `country_id`      |
| `customer`      | `address_id`           | `address`        | `address_id`      |
| `customer`      | `store_id`             | `store`          | `store_id`        |
| `film`          | `language_id`          | `language`       | `language_id`     |
| `film`          | `original_language_id` | `language`       | `language_id`     |
| `film_actor`    | `actor_id`             | `actor`          | `actor_id`        |
| `film_actor`    | `film_id`              | `film`           | `film_id`         |
| `film_category` | `category_id`          | `category`       | `category_id`     |
| `film_category` | `film_id`              | `film`           | `film_id`         |
| `inventory`     | `film_id`              | `film`           | `film_id`         |
| `inventory`     | `store_id`             | `store`          | `store_id`        |
| `payment`       | `customer_id`          | `customer`       | `customer_id`     |
| `payment`       | `rental_id`            | `rental`         | `rental_id`       |
| `payment`       | `staff_id`             | `staff`          | `staff_id`        |
| `rental`        | `customer_id`          | `customer`       | `customer_id`     |
| `rental`        | `inventory_id`         | `inventory`      | `inventory_id`    |
| `rental`        | `staff_id`             | `staff`          | `staff_id`        |
| `staff`         | `address_id`           | `address`        | `address_id`      |
| `staff`         | `store_id`             | `store`          | `store_id`        |
| `store`         | `address_id`           | `address`        | `address_id`      |
| `store`         | `manager_staff_id`     | `staff`          | `staff_id`        |

---

## 7. Key Relational Flows

Based on the observed foreign-key relationships, the following relational flows were identified.

### Customer and Address

```text
Customer
   |
   | address_id
   ↓
Address
   |
   | city_id
   ↓
City
   |
   | country_id
   ↓
Country
```

### Rental Flow

```text
Customer
   |
   | customer_id
   ↓
Rental
   |
   | inventory_id
   ↓
Inventory
   |
   | film_id
   ↓
Film
```

### Payment Flow

```text
Customer
   |
   ↓
Payment
   |
   ├── rental_id → Rental
   |
   └── staff_id → Staff
```

These relationships provide suitable areas for foreign-key and referential-integrity testing.

---

## 8. Initial Row-Count Observations

The following row counts were obtained using SQL `COUNT(*)` queries:

| Table      | Row Count |
| ---------- | --------: |
| `customer` |       599 |
| `film`     |     1,000 |
| `rental`   |    16,044 |
| `payment`  |    16,044 |

These values represent the current state of the local Sakila database used for this project.

---

## 9. Initial Testing Areas

Based on the database exploration, the following areas have been identified for database testing:

* Primary key validation
* Foreign-key validation
* Referential integrity
* NULL and mandatory-field validation
* Duplicate record validation
* Data completeness
* Data consistency between related tables
* JOIN-based validation
* Aggregate data validation
* CRUD validation where appropriate
* Business-rule validation
* Relationship validation
* Record-count validation

---

## 10. Exploration Queries Used

The following SQL queries were used during database exploration:

```sql
USE sakila;

SELECT DATABASE();

SHOW TABLES;

DESCRIBE customer;
```

### Foreign-Key Relationship Exploration

```sql
SELECT
    TABLE_NAME,
    COLUMN_NAME,
    CONSTRAINT_NAME,
    REFERENCED_TABLE_NAME,
    REFERENCED_COLUMN_NAME
FROM INFORMATION_SCHEMA.KEY_COLUMN_USAGE
WHERE TABLE_SCHEMA = 'sakila'
  AND REFERENCED_TABLE_NAME IS NOT NULL
ORDER BY TABLE_NAME, COLUMN_NAME;
```

### Row-Count Exploration

```sql
SELECT COUNT(*) AS customer_count FROM customer;

SELECT COUNT(*) AS film_count FROM film;

SELECT COUNT(*) AS rental_count FROM rental;

SELECT COUNT(*) AS payment_count FROM payment;
```

### Database Object Verification

```sql
SHOW FULL TABLES FROM sakila;
```

---

## 11. Scope Decision

The database testing project will focus on data validation, integrity, relationships, SQL-based verification, and database behavior that can be directly validated using the available Sakila database.

No production database access or application-specific behavior will be claimed.

All test results will be based on actual SQL execution against the local Sakila database.
