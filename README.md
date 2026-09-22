# Database Testing – Sakila MySQL Database

## Project Overview

This project demonstrates practical database testing using the **MySQL Sakila Sample Database**.

The project focuses on validating database structure, schema definitions, primary-key and foreign-key integrity, relational data consistency, SQL-based data validation, aggregate calculations, controlled CRUD operations, default values, and timestamp behavior.

Testing was performed using **MySQL Workbench** against a local instance of the Sakila database.

---

## Objective

The objective of this project is to practice and demonstrate database testing using SQL by:

- Exploring the database structure and relationships.
- Validating table and column definitions.
- Verifying primary-key integrity.
- Verifying foreign-key relationships and referential integrity.
- Validating mandatory fields and NULL handling.
- Validating relationships using SQL JOINs.
- Comparing baseline record counts.
- Validating aggregate calculations.
- Performing controlled INSERT, SELECT, UPDATE, and DELETE operations.
- Verifying configured default values.
- Verifying timestamp behavior.
- Recording actual test execution results.
- Reporting only genuine and reproducible defects.

---

## Application / Database Under Test

| Field | Details |
| ----- | ------- |
| Database | MySQL Sakila Sample Database |
| Database Name | `sakila` |
| Testing Type | Database Testing |
| Test Level | Database / Data Layer |
| Testing Approach | SQL-based validation and data integrity testing |
| Tool | MySQL Workbench |
| Environment | Local MySQL database |
| Tester | Shreya Mandrekar |

---

## Testing Scope

### In Scope

- Database object validation
- Table and view validation
- Column and data-type validation
- Primary-key validation
- Foreign-key validation
- Referential integrity
- Mandatory-field validation
- NULL validation
- Duplicate detection
- Data consistency
- SQL JOIN validation
- Record-count validation
- Aggregate-function validation
- Controlled CRUD operations
- Default-value validation
- Timestamp validation
- Data integrity after controlled database operations

### Out of Scope

- Production database testing
- Performance and load testing
- Security penetration testing
- Backup and recovery testing
- Database administration activities
- Application UI testing
- API testing
- Production business-rule validation not supported by the sample database

---

## Database Exploration

The initial database exploration identified:

- **23 database objects**
- **16 base tables**
- **7 views**

Key tables explored during the project included:

- `customer`
- `film`
- `rental`
- `payment`
- `inventory`
- `address`
- `city`
- `country`
- `store`
- `staff`

Foreign-key relationships were also inspected using the MySQL `INFORMATION_SCHEMA.KEY_COLUMN_USAGE` metadata.

The exploration phase was used to understand the database structure and identify appropriate database-testing areas before test-case design.

---

## Testing Activities

The following database-testing activities were performed:

### 1. Database Structure Validation

Verified the presence and types of database objects using `INFORMATION_SCHEMA.TABLES`.

### 2. Schema Validation

Validated the `customer` table's:

- Column names
- Data types
- NULL/NOT NULL settings
- Primary key
- Index metadata
- Default values
- Auto-increment behavior
- Timestamp configuration

### 3. Primary-Key Validation

Verified that `customer_id` values were:

- Unique
- Non-NULL

### 4. Foreign-Key and Referential Integrity Validation

Validated relationships between:

- Customer and address
- Customer and store
- Rental and customer
- Rental and inventory
- Rental and staff
- Payment and customer
- Payment and rental
- Payment and staff

Orphan-record checks were performed using `LEFT JOIN` queries.

### 5. Relational Data Validation

Used SQL `INNER JOIN` queries to validate connected data across:

- Customer → Address → City → Country
- Customer → Rental → Inventory → Film
- Customer → Rental → Payment

### 6. Record-Count Validation

Baseline counts were validated for selected tables.

| Table | Baseline Count | Executed Count |
| ----- | --------------: | --------------: |
| `customer` | 599 | 599 |
| `film` | 1,000 | 1,000 |
| `rental` | 16,044 | 16,044 |
| `payment` | 16,044 | 16,044 |

### 7. Aggregate Validation

Validated payment data using:

- `COUNT()`
- `SUM()`
- `AVG()`
- `MIN()`
- `MAX()`

Observed payment aggregate results:

| Metric | Result |
| ------ | -----: |
| Payment count | 16,044 |
| Total payment amount | 67,406.56 |
| Average payment amount | 4.201356 |
| Minimum payment amount | 0.00 |
| Maximum payment amount | 11.99 |

### 8. Controlled CRUD Testing

Controlled test records were used to verify:

- INSERT
- SELECT
- UPDATE
- DELETE
- Data validation for an invalid value

Temporary test records were removed after verification.

### 9. Default-Value Validation

Verified that the `customer.active` field automatically received the configured default value of `1` when the field was omitted during insertion.

### 10. Timestamp Validation

Verified that:

- `create_date` was populated during insertion.
- `create_date` remained unchanged after an update.
- `last_update` was automatically updated when the record was modified.

---

## Test Execution Summary

A total of **16 database test cases** were executed.

| Metric | Result |
| ------ | -----: |
| Total Test Cases | 16 |
| Executed | 16 |
| Passed | 16 |
| Failed | 0 |
| Blocked | 0 |
| Defects Identified | 0 |

All results are based on actual SQL execution against the local Sakila database.

---

## Notable Execution Observation

During controlled CRUD testing, an invalid `store_id` value of `9999` was tested.

MySQL rejected the value with an **out-of-range error** because `store_id` is defined as an unsigned `tinyint`.

This was treated as expected data-type/range validation and was **not reported as a foreign-key defect**, because the observed error was an out-of-range value error rather than a foreign-key constraint violation.

---

## Defect Management

Defects were recorded only when an actual, reproducible mismatch was observed during execution.

No genuine defects were identified during the execution of the 16 database test cases.

The project therefore contains a defect report documenting the executed testing results and the absence of identified defects.

---

## Test Data Cleanup

Controlled test records were created only when required for specific test cases.

After verification:

- Temporary customer records were deleted.
- Final verification queries were executed.
- Existing Sakila sample data was not intentionally left modified.
