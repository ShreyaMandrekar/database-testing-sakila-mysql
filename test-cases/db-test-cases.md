# Database Test Cases – Sakila MySQL Database

## 1. Document Information

| Field                  | Details                         |
| ---------------------- | ------------------------------- |
| Project                | Database Testing – Sakila MySQL |
| Application / Database | MySQL Sakila Sample Database    |
| Testing Type           | Database Testing                |
| Test Level             | Database / Data Layer           |
| Tester                 | Shreya Mandrekar                |
| Tool                   | MySQL Workbench                 |
| Database               | `sakila`                        |
| Document               | Database Test Cases             |

---

## 2. Test Case Execution Fields

The following fields are intentionally left blank before execution:

* Actual Result
* Status
* Defect ID
* Comments

These fields will be updated only after the corresponding SQL queries are actually executed.

---

# 3. Database Structure Test Cases

## TC-DB-001 – Verify database objects and object types

**Scenario ID:** TS-DB-001
**Priority:** High

**Preconditions:**

* MySQL Server is running.
* `sakila` database is installed and accessible.

**Test Data:**
Database name: `sakila`

**SQL / Query Steps:**

```sql
SELECT
    TABLE_TYPE,
    COUNT(*) AS object_count
FROM INFORMATION_SCHEMA.TABLES
WHERE TABLE_SCHEMA = 'sakila'
GROUP BY TABLE_TYPE
ORDER BY TABLE_TYPE;
```

**Expected Result:**

The query should identify:

* 16 `BASE TABLE` objects
* 7 `VIEW` objects
* 23 database objects in total

No unexpected object type should be present.

**Actual Result:**

The SQL query returned the following database object types:

| TABLE_TYPE | object_count |
|---|---:|
| BASE TABLE | 16 |
| VIEW | 7 |

The Sakila database contains 16 base tables and 7 views, for a total of 23 database objects.

**Status:**
PASS

**Defect ID:**
N/A

**Comments:**
The observed database object types and counts matched the expected result. No unexpected database object type was identified.

---

## TC-DB-002 – Verify customer table columns and constraints

**Scenario ID:** TS-DB-002
**Priority:** High

**Preconditions:**

* `sakila` database is accessible.

**Test Data:**
Table: `customer`

**SQL / Query Steps:**

```sql
SELECT
    COLUMN_NAME,
    DATA_TYPE,
    IS_NULLABLE,
    COLUMN_KEY,
    COLUMN_DEFAULT,
    EXTRA
FROM INFORMATION_SCHEMA.COLUMNS
WHERE TABLE_SCHEMA = 'sakila'
  AND TABLE_NAME = 'customer'
ORDER BY ORDINAL_POSITION;
```

**Expected Result:**

The `customer` table should contain the expected columns and definitions, including:

| Column        | Expected Data Type | Nullable | Key / Constraint                   |
| ------------- | ------------------ | -------- | ---------------------------------- |
| `customer_id` | `smallint`         | NO       | Primary Key, Auto Increment        |
| `store_id`    | `tinyint`          | NO       | Indexed / Foreign Key              |
| `first_name`  | `varchar`          | NO       | —                                  |
| `last_name`   | `varchar`          | NO       | Indexed                            |
| `email`       | `varchar`          | YES      | —                                  |
| `address_id`  | `smallint`         | NO       | Indexed / Foreign Key              |
| `active`      | `tinyint`          | NO       | Default `1`                        |
| `create_date` | `datetime`         | NO       | —                                  |
| `last_update` | `timestamp`        | YES      | Default/current timestamp behavior |

The column definitions should match the observed Sakila schema.

**Actual Result:**

The query returned 9 rows for the `customer` table with the following column definitions:

| Column | Data Type | Nullable | Column Key | Default | Extra |
|---|---|---|---|---|---|
| `customer_id` | smallint | NO | PRI | NULL | auto_increment |
| `store_id` | tinyint | NO | MUL | NULL | |
| `first_name` | varchar | NO | | NULL | |
| `last_name` | varchar | NO | MUL | NULL | |
| `email` | varchar | YES | | NULL | |
| `address_id` | smallint | NO | MUL | NULL | |
| `active` | tinyint | NO | | 1 | |
| `create_date` | datetime | NO | | NULL | |
| `last_update` | timestamp | YES | | CURRENT_TIMESTAMP | DEFAULT_GENERATED on update CURRENT_TIMESTAMP |

The observed column definitions, nullable settings, primary key, indexes, default value, and auto-update timestamp behavior matched the expected customer table schema.

**Status:**
PASS

**Defect ID:**
N/A

**Comments:**
The `customer` table returned all 9 expected columns with the expected data types and constraint-related metadata. `customer_id` was identified as the primary key with auto-increment, `active` had a default value of 1, and `last_update` had the expected current timestamp and on-update behavior.

---

# 4. Primary Key Test Cases

## TC-DB-003 – Verify customer primary key uniqueness and non-null values

**Scenario ID:** TS-DB-003, TS-DB-004
**Priority:** High

**Preconditions:**

* `customer` table is accessible.

**Test Data:**
Table: `customer`
Primary Key: `customer_id`

**SQL / Query Steps:**

```sql
SELECT
    COUNT(*) AS total_rows,
    COUNT(DISTINCT customer_id) AS distinct_customer_ids,
    COUNT(*) - COUNT(customer_id) AS null_customer_ids
FROM customer;
```

**Expected Result:**

* `total_rows` should equal `distinct_customer_ids`.
* `null_customer_ids` should be `0`.
* No duplicate or NULL `customer_id` values should exist.

**Actual Result:**

The query returned the following result:

| total_rows | distinct_customer_ids | null_customer_ids |
|---:|---:|---:|
| 599 | 599 | 0 |

The total number of customer records matched the number of distinct `customer_id` values, and no NULL `customer_id` values were found.

**Status:**
PASS

**Defect ID:**
N/A

**Comments:**
The `customer_id` primary key contains unique and non-NULL values across all 599 customer records. No primary key integrity issue was identified.

---

## TC-DB-004 – Verify mandatory customer fields do not contain NULL

**Scenario ID:** TS-DB-009
**Priority:** High

**Preconditions:**

* `customer` table is accessible.

**Test Data:**
Mandatory fields observed in the customer schema.

**SQL / Query Steps:**

```sql
SELECT COUNT(*) AS invalid_null_records
FROM customer
WHERE customer_id IS NULL
   OR store_id IS NULL
   OR first_name IS NULL
   OR last_name IS NULL
   OR address_id IS NULL
   OR active IS NULL
   OR create_date IS NULL;
```

**Expected Result:**

`invalid_null_records` should be `0`.

No mandatory customer field should contain NULL.

**Actual Result:**

**Status:**

**Defect ID:**

**Comments:**

---

# 5. Foreign Key and Referential Integrity Test Cases

## TC-DB-005 – Verify customer foreign-key references

**Scenario ID:** TS-DB-007, TS-DB-008
**Priority:** High

**Preconditions:**

* `customer`, `address`, and `store` tables are accessible.

**Test Data:**
Foreign-key relationships:

* `customer.address_id → address.address_id`
* `customer.store_id → store.store_id`

**SQL / Query Steps:**

```sql
SELECT COUNT(*) AS orphan_customer_addresses
FROM customer c
LEFT JOIN address a
    ON c.address_id = a.address_id
WHERE a.address_id IS NULL;

SELECT COUNT(*) AS orphan_customer_stores
FROM customer c
LEFT JOIN store s
    ON c.store_id = s.store_id
WHERE s.store_id IS NULL;
```

**Expected Result:**

Both queries should return `0`.

Every customer should reference an existing address and store.

**Actual Result:**

**Status:**

**Defect ID:**

**Comments:**

---

## TC-DB-006 – Verify rental foreign-key integrity

**Scenario ID:** TS-DB-007, TS-DB-008
**Priority:** High

**Preconditions:**

* `rental`, `customer`, `inventory`, and `staff` tables are accessible.

**Test Data:**
Foreign-key relationships:

* `rental.customer_id → customer.customer_id`
* `rental.inventory_id → inventory.inventory_id`
* `rental.staff_id → staff.staff_id`

**SQL / Query Steps:**

```sql
SELECT COUNT(*) AS orphan_rental_records
FROM rental r
LEFT JOIN customer c
    ON r.customer_id = c.customer_id
LEFT JOIN inventory i
    ON r.inventory_id = i.inventory_id
LEFT JOIN staff s
    ON r.staff_id = s.staff_id
WHERE c.customer_id IS NULL
   OR i.inventory_id IS NULL
   OR s.staff_id IS NULL;
```

**Expected Result:**

`orphan_rental_records` should be `0`.

Every rental should reference an existing customer, inventory item, and staff member.

**Actual Result:**

**Status:**

**Defect ID:**

**Comments:**

---

## TC-DB-007 – Verify payment foreign-key integrity

**Scenario ID:** TS-DB-007, TS-DB-008
**Priority:** High

**Preconditions:**

* `payment`, `customer`, `rental`, and `staff` tables are accessible.

**Test Data:**
Foreign-key relationships:

* `payment.customer_id → customer.customer_id`
* `payment.rental_id → rental.rental_id`
* `payment.staff_id → staff.staff_id`

**SQL / Query Steps:**

```sql
SELECT COUNT(*) AS orphan_payment_records
FROM payment p
LEFT JOIN customer c
    ON p.customer_id = c.customer_id
LEFT JOIN rental r
    ON p.rental_id = r.rental_id
LEFT JOIN staff s
    ON p.staff_id = s.staff_id
WHERE c.customer_id IS NULL
   OR r.rental_id IS NULL
   OR s.staff_id IS NULL;
```

**Expected Result:**

`orphan_payment_records` should be `0`.

Every payment should reference an existing customer, rental, and staff member.

**Actual Result:**

**Status:**

**Defect ID:**

**Comments:**

---

# 6. JOIN and Data Consistency Test Cases

## TC-DB-008 – Verify customer, address, city and country relationship

**Scenario ID:** TS-DB-016
**Priority:** High

**Preconditions:**

* Related customer, address, city, and country tables are accessible.

**Test Data:**
Relationship chain:

`customer → address → city → country`

**SQL / Query Steps:**

```sql
SELECT
    c.customer_id,
    c.first_name,
    c.last_name,
    a.address,
    ci.city,
    co.country
FROM customer c
JOIN address a
    ON c.address_id = a.address_id
JOIN city ci
    ON a.city_id = ci.city_id
JOIN country co
    ON ci.country_id = co.country_id
ORDER BY c.customer_id
LIMIT 10;
```

**Expected Result:**

Each returned customer should display:

* Valid customer information
* A matching address
* A matching city
* A matching country

No broken relationship should result in missing joined records.

**Actual Result:**

**Status:**

**Defect ID:**

**Comments:**

---

## TC-DB-009 – Verify customer, rental, inventory and film relationship

**Scenario ID:** TS-DB-017
**Priority:** High

**Preconditions:**

* Related tables are accessible.

**Test Data:**
Relationship chain:

`customer → rental → inventory → film`

**SQL / Query Steps:**

```sql
SELECT
    c.customer_id,
    c.first_name,
    c.last_name,
    r.rental_id,
    i.inventory_id,
    f.film_id,
    f.title
FROM customer c
JOIN rental r
    ON c.customer_id = r.customer_id
JOIN inventory i
    ON r.inventory_id = i.inventory_id
JOIN film f
    ON i.film_id = f.film_id
ORDER BY c.customer_id, r.rental_id
LIMIT 10;
```

**Expected Result:**

Each returned rental should have:

* A valid customer
* A valid rental record
* A valid inventory record
* A valid film record

The film title should correspond to the referenced film record.

**Actual Result:**

**Status:**

**Defect ID:**

**Comments:**

---

## TC-DB-010 – Verify customer, rental and payment relationship

**Scenario ID:** TS-DB-018
**Priority:** High

**Preconditions:**

* Related tables are accessible.

**Test Data:**
Relationship chain:

`customer → rental → payment`

**SQL / Query Steps:**

```sql
SELECT
    c.customer_id,
    c.first_name,
    c.last_name,
    r.rental_id,
    p.payment_id,
    p.amount
FROM customer c
JOIN rental r
    ON c.customer_id = r.customer_id
JOIN payment p
    ON r.rental_id = p.rental_id
ORDER BY c.customer_id, r.rental_id
LIMIT 10;
```

**Expected Result:**

Each returned payment should correspond to an existing rental and customer.

The `payment.rental_id` should match the related `rental.rental_id`.

**Actual Result:**

**Status:**

**Defect ID:**

**Comments:**

---

# 7. Record Count Test Cases

## TC-DB-011 – Verify baseline record counts for selected tables

**Scenario ID:** TS-DB-019
**Priority:** Medium

**Preconditions:**

* No controlled data modification has been performed before execution.

**Test Data:**
Tables:

* `customer`
* `film`
* `rental`
* `payment`

**SQL / Query Steps:**

```sql
SELECT COUNT(*) AS customer_count FROM customer;

SELECT COUNT(*) AS film_count FROM film;

SELECT COUNT(*) AS rental_count FROM rental;

SELECT COUNT(*) AS payment_count FROM payment;
```

**Expected Result:**

The counts should match the currently observed Sakila baseline:

| Table      | Expected Count |
| ---------- | -------------: |
| `customer` |            599 |
| `film`     |           1000 |
| `rental`   |          16044 |
| `payment`  |          16044 |

If the database has been intentionally modified before execution, the actual baseline should be documented instead of automatically treating a count difference as a defect.

**Actual Result:**

**Status:**

**Defect ID:**

**Comments:**

---

## TC-DB-012 – Verify COUNT consistency for payment records

**Scenario ID:** TS-DB-021
**Priority:** Medium

**Preconditions:**

* `payment` table is accessible.

**Test Data:**
Table: `payment`

**SQL / Query Steps:**

```sql
SELECT
    COUNT(*) AS total_rows,
    SUM(1) AS summed_rows
FROM payment;
```

**Expected Result:**

`total_rows` should equal `summed_rows`.

The two methods should produce the same number of payment records.

**Actual Result:**

**Status:**

**Defect ID:**

**Comments:**

---

# 8. Aggregate Function Test Cases

## TC-DB-013 – Verify payment aggregate calculations

**Scenario ID:** TS-DB-022, TS-DB-023, TS-DB-024
**Priority:** Medium

**Preconditions:**

* `payment` table is accessible.

**Test Data:**
Column: `payment.amount`

**SQL / Query Steps:**

### Total validation

```sql
SELECT
    SUM(amount) AS total_payment,
    SUM(customer_total) AS grouped_total
FROM (
    SELECT
        customer_id,
        SUM(amount) AS customer_total
    FROM payment
    GROUP BY customer_id
) AS customer_totals;
```

### Average validation

```sql
SELECT
    AVG(amount) AS calculated_average,
    SUM(amount) / COUNT(amount) AS sum_divided_by_count
FROM payment;
```

### Minimum and maximum validation

```sql
SELECT
    MIN(amount) AS minimum_amount,
    MAX(amount) AS maximum_amount
FROM payment;
```

**Expected Result:**

* The total payment calculated directly should match the grouped customer total.
* `AVG(amount)` should match `SUM(amount) / COUNT(amount)`.
* `MIN(amount)` should represent the lowest payment amount.
* `MAX(amount)` should represent the highest payment amount.
* Aggregate results should be numeric and internally consistent.

**Actual Result:**

**Status:**

**Defect ID:**

**Comments:**

---

# 9. Default Value Test Cases

## TC-DB-014 – Verify default value of customer active field

**Scenario ID:** TS-DB-030
**Priority:** Medium

**Preconditions:**

* `customer` table is accessible.
* A controlled transaction can be used for test data.

**Test Data:**

Temporary customer record:

* `store_id`: `1`
* `first_name`: `QA`
* `last_name`: `DB_TEST`
* `email`: `qa.dbtest@example.com`
* `address_id`: `1`
* `create_date`: current timestamp
* `active`: intentionally omitted

**SQL / Query Steps:**

```sql
START TRANSACTION;

INSERT INTO customer
(
    store_id,
    first_name,
    last_name,
    email,
    address_id,
    create_date
)
VALUES
(
    1,
    'QA',
    'DB_TEST',
    'qa.dbtest@example.com',
    1,
    NOW()
);

SET @test_customer_id = LAST_INSERT_ID();

SELECT
    customer_id,
    active
FROM customer
WHERE customer_id = @test_customer_id;

ROLLBACK;
```

**Expected Result:**

* The test record should be inserted successfully.
* The `active` field should automatically receive its defined default value of `1`.
* The transaction should be rolled back after verification.

**Actual Result:**

**Status:**

**Defect ID:**

**Comments:**

---

# 10. Controlled CRUD Test Cases

## TC-DB-015 – Verify controlled customer CRUD operations

**Scenario ID:** TS-DB-025, TS-DB-026, TS-DB-027, TS-DB-028, TS-DB-029, TS-DB-032
**Priority:** High

**Preconditions:**

* `customer` table is accessible.
* `store_id = 1` and `address_id = 1` exist.
* Test is executed inside a transaction.
* No production database is involved.

**Test Data:**

Temporary customer record:

* First Name: `QA`
* Last Name: `DB_TEST`
* Email: `qa.dbtest@example.com`
* Store ID: `1`
* Address ID: `1`

**SQL / Query Steps:**

### Step 1 – Insert

```sql
START TRANSACTION;

INSERT INTO customer
(
    store_id,
    first_name,
    last_name,
    email,
    address_id,
    create_date
)
VALUES
(
    1,
    'QA',
    'DB_TEST',
    'qa.dbtest@example.com',
    1,
    NOW()
);

SET @test_customer_id = LAST_INSERT_ID();
```

### Step 2 – Retrieve

```sql
SELECT
    customer_id,
    first_name,
    last_name,
    email,
    store_id,
    address_id,
    active
FROM customer
WHERE customer_id = @test_customer_id;
```

### Step 3 – Update

```sql
UPDATE customer
SET last_name = 'DB_TEST_UPDATED'
WHERE customer_id = @test_customer_id;

SELECT
    customer_id,
    first_name,
    last_name,
    email
FROM customer
WHERE customer_id = @test_customer_id;
```

### Step 4 – Delete

```sql
DELETE FROM customer
WHERE customer_id = @test_customer_id;

SELECT COUNT(*) AS remaining_record
FROM customer
WHERE customer_id = @test_customer_id;
```

### Step 5 – Rollback

```sql
ROLLBACK;
```

**Expected Result:**

* A valid test record should be inserted successfully.
* The inserted record should be retrievable using its generated `customer_id`.
* The `last_name` should update successfully.
* The updated value should be returned by the SELECT query.
* The test record should be deleted successfully.
* `remaining_record` should be `0` before rollback.
* The transaction should be rolled back after execution so the test record does not remain in the database.
* No existing Sakila production/sample record should be unintentionally modified or deleted.

**Actual Result:**

**Status:**

**Defect ID:**

**Comments:**

---

# 11. Timestamp Test Cases

## TC-DB-016 – Verify customer timestamp behavior

**Scenario ID:** TS-DB-031
**Priority:** Medium

**Preconditions:**

* `customer` table is accessible.
* Controlled transaction can be used.

**Test Data:**
Temporary customer record.

**SQL / Query Steps:**

```sql
START TRANSACTION;

INSERT INTO customer
(
    store_id,
    first_name,
    last_name,
    email,
    address_id,
    create_date
)
VALUES
(
    1,
    'QA',
    'TIMESTAMP_TEST',
    'qa.timestamp@example.com',
    1,
    NOW()
);

SET @test_customer_id = LAST_INSERT_ID();

SELECT
    customer_id,
    create_date,
    last_update
FROM customer
WHERE customer_id = @test_customer_id;
```

Wait approximately one second before continuing.

```sql
UPDATE customer
SET last_name = 'TIMESTAMP_UPDATED'
WHERE customer_id = @test_customer_id;

SELECT
    customer_id,
    create_date,
    last_update
FROM customer
WHERE customer_id = @test_customer_id;

ROLLBACK;
```

**Expected Result:**

* `create_date` should be populated when the test record is inserted.
* `last_update` should be populated.
* After the update operation, `last_update` should reflect the update time.
* The updated timestamp should not be earlier than the original timestamp.
* The transaction should be rolled back after verification.

**Actual Result:**

**Status:**

**Defect ID:**

**Comments:**

---

# 12. Test Case Traceability

| Test Case ID | Scenario ID(s)                                                   | Testing Area            |
| ------------ | ---------------------------------------------------------------- | ----------------------- |
| TC-DB-001    | TS-DB-001                                                        | Database Structure      |
| TC-DB-002    | TS-DB-002                                                        | Schema / Columns        |
| TC-DB-003    | TS-DB-003, TS-DB-004                                             | Primary Key             |
| TC-DB-004    | TS-DB-009                                                        | Mandatory Fields        |
| TC-DB-005    | TS-DB-007, TS-DB-008                                             | Customer Foreign Keys   |
| TC-DB-006    | TS-DB-007, TS-DB-008                                             | Rental Foreign Keys     |
| TC-DB-007    | TS-DB-007, TS-DB-008                                             | Payment Foreign Keys    |
| TC-DB-008    | TS-DB-016                                                        | JOIN / Data Consistency |
| TC-DB-009    | TS-DB-017                                                        | JOIN / Data Consistency |
| TC-DB-010    | TS-DB-018                                                        | JOIN / Data Consistency |
| TC-DB-011    | TS-DB-019                                                        | Record Counts           |
| TC-DB-012    | TS-DB-021                                                        | COUNT Validation        |
| TC-DB-013    | TS-DB-022, TS-DB-023, TS-DB-024                                  | Aggregate Validation    |
| TC-DB-014    | TS-DB-030                                                        | Default Values          |
| TC-DB-015    | TS-DB-025, TS-DB-026, TS-DB-027, TS-DB-028, TS-DB-029, TS-DB-032 | CRUD / Integrity        |
| TC-DB-016    | TS-DB-031                                                        | Timestamp Validation    |

---

# 13. Execution Notes

* Test cases will be executed using MySQL Workbench against the local Sakila database.
* Actual results will be recorded only after query execution.
* A test case will be marked **PASS** when the observed result matches the expected result.
* A test case will be marked **FAIL** only when an actual mismatch is observed and confirmed.
* A defect ID will be assigned only for a genuine, reproducible failure.
* Controlled INSERT, UPDATE, and DELETE operations will be executed inside transactions and rolled back after verification where applicable.
* Existing Sakila sample data should not be intentionally left modified by the test execution.
* Differences in baseline record counts should be investigated before being classified as defects because the database may have been modified after the initial exploration.
* No test result should be fabricated or assumed before execution.

---

## 14. Status Legend

| Status  | Meaning                                                                 |
| ------- | ----------------------------------------------------------------------- |
| PASS    | Actual result matches expected result                                   |
| FAIL    | Actual result does not match expected result                            |
| BLOCKED | Test could not be executed because a required condition was unavailable |
| NOT RUN | Test case has not yet been executed                                     |
