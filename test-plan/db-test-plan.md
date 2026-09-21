# Database Test Plan – Sakila

## 1. Document Information

| Field        | Details                 |
| ------------ | ----------------------- |
| Project      | Sakila Database Testing |
| Database     | Sakila                  |
| DBMS         | MySQL                   |
| Testing Type | Database Testing        |
| Tool         | MySQL Workbench         |
| Tester       | Shreya Mandrekar        |

---

## 2. Objective

The objective of database testing is to verify the accuracy, integrity, consistency, and reliability of data stored in the Sakila relational database.

Testing will focus on validating database structure, relationships, data integrity, and SQL-based business data checks.

---

## 3. Scope

### In Scope

* Table and column validation
* Primary key validation
* Foreign key validation
* Referential integrity
* NULL and mandatory-field validation
* Duplicate data validation
* Data completeness
* Data consistency
* Relationship validation using JOINs
* Record-count validation
* Aggregate data validation
* CRUD validation where appropriate
* Data validation after controlled database operations

### Out of Scope

* Production database testing
* Database performance/load testing
* Security penetration testing
* Backup and recovery testing
* Database administration activities
* Application UI testing
* Application API testing
* Testing database behavior that cannot be directly verified using the available Sakila database

---

## 4. Test Approach

Database testing will be performed using SQL queries in MySQL Workbench.

The testing approach will include:

1. Review the database structure and relationships.
2. Identify suitable database test scenarios.
3. Create detailed database test cases.
4. Execute SQL queries against the Sakila database.
5. Compare actual database results with expected results.
6. Record test execution results.
7. Document only genuine and reproducible database defects.
8. Avoid modifying existing data unless a test case specifically requires a controlled operation.

---

## 5. Testing Areas

The following areas will be covered:

### 5.1 Structural Validation

* Verify required tables exist.
* Verify expected columns exist.
* Verify primary keys.
* Verify data types.
* Verify NULL and NOT NULL constraints.
* Verify default values where applicable.

### 5.2 Primary Key Validation

* Verify primary key values are unique.
* Verify primary key values are not NULL.
* Verify primary key relationships where applicable.

### 5.3 Foreign Key Validation

* Verify foreign-key relationships.
* Verify referenced records exist.
* Verify referential integrity between related tables.

### 5.4 Data Validation

* Verify mandatory fields contain valid data.
* Verify NULL values occur only where permitted.
* Verify duplicate records where uniqueness is expected.
* Verify data values against applicable constraints.

### 5.5 Relationship Validation

* Validate related records using SQL JOIN queries.
* Verify that child records reference valid parent records.
* Verify consistency between related tables.

### 5.6 Record Count Validation

* Compare record counts where meaningful.
* Verify expected relationships between related record sets.

### 5.7 Aggregate Validation

* Validate calculated values using SQL aggregate functions such as:

  * `COUNT()`
  * `SUM()`
  * `AVG()`
  * `MIN()`
  * `MAX()`

### 5.8 CRUD Validation

Where appropriate and safe for the sample database:

* Create records
* Read records
* Update records
* Delete records

Controlled test data will be used for modification-based testing to avoid unnecessary changes to existing sample data.

---

## 6. Test Design Techniques

The following techniques may be used where applicable:

* Positive testing
* Negative testing
* Boundary Value Analysis
* Equivalence Partitioning
* NULL value testing
* Duplicate value testing
* Referential integrity testing
* Data consistency testing
* SQL JOIN validation
* Aggregate validation

Only techniques that are relevant to a specific database test case will be applied.

---

## 7. Test Environment

| Item          | Details                |
| ------------- | ---------------------- |
| Database      | Sakila                 |
| DBMS          | MySQL                  |
| Client Tool   | MySQL Workbench        |
| Database Type | Relational Database    |
| Environment   | Local database         |
| Testing Data  | Sakila sample database |

---

## 8. Entry Criteria

Database testing can begin when:

* Sakila database is available and accessible.
* MySQL Workbench can connect to the database.
* Database structure has been explored.
* Relevant tables and relationships have been identified.
* Test scenarios have been defined.
* Required SQL queries can be executed successfully.

---

## 9. Exit Criteria

Database testing can be considered complete when:

* Planned database test cases have been executed.
* Actual results have been recorded.
* Failed test cases have been investigated.
* Genuine reproducible defects have been documented where applicable.
* Test execution results have been reviewed.
* The final execution status has been documented.

---

## 10. Defect Handling

A database issue will be considered a defect only when:

* The observed behavior differs from the expected result.
* The issue is reproducible.
* Sufficient evidence is available to support the finding.

For genuine defects, the following information will be documented:

* Defect ID
* Test Case ID
* Description
* Expected Result
* Actual Result
* Severity
* Priority
* Reproduction Steps
* Evidence
* Status

Issues will not be reported as defects based only on assumptions or personal expectations.

---

## 11. Test Data Management

The Sakila sample data will primarily be used for read-only validation.

For test cases requiring INSERT, UPDATE, or DELETE operations:

* Test data will be controlled.
* Existing sample records will not be modified unnecessarily.
* Data changes will be performed only when required by the test case.
* Test data will be restored where appropriate.

---

## 12. Risks and Assumptions

### Risks

* Local database data may differ from other Sakila installations.
* Destructive database operations may affect subsequent test results.
* Sample data may not represent all real-world business scenarios.

### Assumptions

* The local Sakila database is available and accessible through MySQL Workbench.
* SQL permissions required for planned testing are available.
* Test results will be based on the current local database state.

---

## 13. Deliverables

The following project artifacts will be created:

* Database exploration notes
* Database test plan
* Database test scenarios
* Database test cases
* Database test execution results
* Defect reports, if genuine defects are identified
* Project README

---

## 14. Conclusion

This test plan defines the approach for validating the Sakila MySQL database using SQL and MySQL Workbench.

The testing will focus on database structure, data integrity, relationships, consistency, and SQL-based validation using the available local sample database.

All test results will be based on actual SQL execution, and only genuine reproducible issues will be documented as defects.
