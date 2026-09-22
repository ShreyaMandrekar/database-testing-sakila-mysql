# Database Test Scenarios – Sakila

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

## 2. Test Scenarios

### Database Structure

**TS-DB-001:** Verify that required database tables and views are available.

**TS-DB-002:** Verify that table columns have the expected data types and constraints.

---

### Primary Key Validation

**TS-DB-003:** Verify that primary key columns contain unique values.

**TS-DB-004:** Verify that primary key columns do not contain NULL values.

**TS-DB-005:** Verify that primary key values are generated correctly for applicable auto-increment columns.

---

### Foreign Key Validation

**TS-DB-006:** Verify that defined foreign-key relationships exist between related tables.

**TS-DB-007:** Verify that foreign-key columns contain valid references to parent records.

**TS-DB-008:** Verify referential integrity between parent and child tables.

---

### NULL and Mandatory Field Validation

**TS-DB-009:** Verify that mandatory columns do not contain NULL values.

**TS-DB-010:** Verify that columns allowing NULL values contain NULL only where permitted.

---

### Duplicate Data Validation

**TS-DB-011:** Verify that records are not duplicated where uniqueness is expected.

**TS-DB-012:** Verify duplicate detection for selected business data fields where applicable.

---

### Data Consistency Validation

**TS-DB-013:** Verify consistency of related data across connected tables.

**TS-DB-014:** Verify that child records correspond to valid parent records.

**TS-DB-015:** Verify that related records return consistent data when retrieved using JOIN queries.

---

### JOIN-Based Validation

**TS-DB-016:** Verify customer and address data using JOIN validation.

**TS-DB-017:** Verify customer, rental, inventory, and film relationships using JOIN validation.

**TS-DB-018:** Verify customer, rental, and payment relationships using JOIN validation.

---

### Record Count Validation

**TS-DB-019:** Verify record counts for selected database tables.

**TS-DB-020:** Verify record-count consistency between related tables where an applicable relationship or business rule exists.

---

### Aggregate Data Validation

**TS-DB-021:** Verify record totals using `COUNT()`.

**TS-DB-022:** Verify calculated totals using `SUM()` where applicable.

**TS-DB-023:** Verify average values using `AVG()` where applicable.

**TS-DB-024:** Verify minimum and maximum values using `MIN()` and `MAX()` where applicable.

---

### CRUD Validation

**TS-DB-025:** Verify that valid records can be inserted where controlled test data is appropriate.

**TS-DB-026:** Verify that inserted records can be retrieved correctly.

**TS-DB-027:** Verify that applicable records can be updated correctly using controlled test data.

**TS-DB-028:** Verify that controlled test records can be deleted correctly.

---

### Data Integrity and Business Rule Validation

**TS-DB-029:** Verify that database constraints prevent invalid data where applicable.

**TS-DB-030:** Verify that default values are applied correctly where defined.

**TS-DB-031:** Verify that timestamp fields are populated and updated correctly where applicable.

**TS-DB-032:** Verify data integrity after controlled database operations.

---

## 3. Scenario Coverage

The above scenarios cover:

* Database structure validation
* Primary key validation
* Foreign-key validation
* Referential integrity
* NULL and mandatory-field validation
* Duplicate data validation
* Data consistency
* JOIN-based validation
* Record-count validation
* Aggregate validation
* CRUD validation
* Constraint validation
* Default-value validation
* Timestamp validation
* Data integrity validation

---

## 4. Execution Note

These scenarios define the planned database testing coverage.

Detailed test cases will be created from the applicable scenarios before execution. Actual results and execution status will be recorded only after the corresponding SQL queries are executed against the local Sakila database.

No test result or defect status is assigned at the scenario stage.
