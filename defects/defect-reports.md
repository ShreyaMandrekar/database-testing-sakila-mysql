# Defect Reports – Sakila MySQL Database

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
| Document               | Defect Reports                 |

---

## 2. Defect Summary

A total of **16 database test cases** were executed against the local Sakila MySQL database.

| Metric | Result |
| ------ | ------: |
| Total Test Cases Executed | 16 |
| Passed | 16 |
| Failed | 0 |
| Blocked | 0 |
| Defects Identified | 0 |

---

## 3. Defect Status

No genuine, reproducible defects were identified during the execution of the database test cases.

The executed test cases covered database structure, schema validation, primary-key integrity, foreign-key integrity, NULL validation, relational data consistency, JOIN validation, record counts, aggregate calculations, default values, controlled CRUD operations, and timestamp behavior.

All observed results matched the expected results for the executed test cases.

---

## 4. Defect Register

| Defect ID | Related Test Case | Description | Severity | Priority | Status |
| --------- | ----------------- | ----------- | -------- | -------- | ------ |
| N/A | N/A | No defects identified during execution. | N/A | N/A | N/A |

---

## 5. Notable Validation Observations

During controlled database testing, an invalid `store_id` value of `9999` was tested as part of **TC-DB-015**.

The INSERT operation was rejected by MySQL with an out-of-range value error for the `store_id` column.

This behavior was treated as expected data-type/range validation and was **not reported as a foreign-key defect**, because the observed error occurred due to the valid range restriction of the `tinyint unsigned` column.

---

## 6. Test Data Cleanup

Temporary records created during controlled database testing were removed after verification.

The controlled records used for default-value, CRUD, and timestamp testing were successfully deleted, and final verification queries confirmed that the temporary test records were no longer present.

Existing Sakila sample data was not intentionally left modified by the test execution.

---

## 7. Conclusion

No defects were identified during the execution of the 16 database test cases.

The executed test cases produced results consistent with their expected outcomes, and all controlled test data was cleaned up after execution.

This document records the defect status based only on the database test execution performed against the local Sakila MySQL database.

---
