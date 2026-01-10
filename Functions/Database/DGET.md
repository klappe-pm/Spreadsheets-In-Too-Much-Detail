---
categories:
- spreadsheet-functions
subCategories:
- excel
- sheets
topics:
- database
subTopics:
- criteria-range
- single-value-lookup
- data-retrieval
- exact-match
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- database-get
- database-lookup
tags:
- database-functions
- lookup
- criteria-based
- single-record
---

# DGET

## Description

DGET extracts a single value from a database column where exactly one record matches the specified criteria. Unlike DSUM or DAVERAGE which aggregate multiple matching records, DGET is designed for precise lookups where you expect one and only one match. If zero records match, DGET returns #VALUE!; if multiple records match, it returns #NUM!. This strictness makes DGET valuable for ensuring data integrity in lookup scenarios.

The **criteria range** mechanism works identically to other database functions. Create a separate area with column headers matching your database headers exactly, then place criteria values below. Same-row criteria combine with AND logic, while different-row criteria create OR logic. For DGET specifically, your criteria should be precise enough to isolate exactly one record. Think of it as a multi-criteria lookup that validates uniqueness.

DGET serves a different purpose than VLOOKUP or INDEX/MATCH. While those functions find the first match, DGET confirms that only one match exists and returns an error otherwise. This behavior is valuable in data validation scenarios where duplicate matches indicate a data quality problem. DGET is also useful when your lookup key spans multiple columns (e.g., find the value where Region="East" AND Year=2024 AND Product="Widget").

In the database function framework, your data must be organized with headers in the first row and records below. The field parameter specifies which column's value to return and can be a quoted header name, column number (starting at 1), or cell reference. DGET returns the actual cell value from the matching record, whether it is a number, text, date, or even an error value stored in that cell.

## Syntax

> [!NOTE] Syntax
> ```
> DGET(database, field, criteria)
> ```

| Parameter | Description | Required |
|-----------|-------------|----------|
| database | The range containing your structured data, including column headers in the first row and data records in subsequent rows | Yes |
| field | The column from which to retrieve the value, specified as a column header name in quotes, a column number (1-based), or a cell reference | Yes |
| criteria | A range containing column headers and criteria values that define which record to match (must match exactly one record) | Yes |

## Examples

### Example 1: Basic Single Value Retrieval

**Database (A1:D6):**
| Product | Category | Price | Quantity |
|---------|----------|-------|----------|
| Widget A | Electronics | 25 | 100 |
| Widget B | Electronics | 35 | 150 |
| Gadget X | Home | 45 | 80 |
| Gadget Y | Home | 55 | 60 |
| Device Z | Electronics | 65 | 200 |

**Criteria (F1:F2):**
| Product |
|---------|
| Widget A |

**Formula:**
```
=DGET(A1:D6, "Price", F1:F2)
```

**Result:** `25`

**Explanation:** Only one product named "Widget A" exists. DGET returns its price of 25.

---

### Example 2: Multiple Criteria Lookup

**Database (A1:D6):** Same as Example 1

**Criteria (F1:G2):**
| Category | Price |
|----------|-------|
| Electronics | 35 |

**Formula:**
```
=DGET(A1:D6, "Product", F1:G2)
```

**Result:** `Widget B`

**Explanation:** Only Widget B is both Electronics AND has a price of 35. DGET returns the product name.

---

### Example 3: Using Column Number

**Database (A1:D6):** Same as Example 1

**Criteria (F1:F2):**
| Product |
|---------|
| Device Z |

**Formula:**
```
=DGET(A1:D6, 4, F1:F2)
```

**Result:** `200`

**Explanation:** Column 4 is Quantity. Returns the quantity for Device Z.

---

### Example 4: Error When Multiple Matches (NUM!)

**Database (A1:D6):** Same as Example 1

**Criteria (F1:F2):**
| Category |
|----------|
| Electronics |

**Formula:**
```
=DGET(A1:D6, "Price", F1:F2)
```

**Result:** `#NUM!`

**Explanation:** Three products are Electronics (Widget A, Widget B, Device Z). DGET cannot return multiple values and throws #NUM! error.

---

### Example 5: Error When No Matches (VALUE!)

**Database (A1:D6):** Same as Example 1

**Criteria (F1:F2):**
| Category |
|----------|
| Outdoor |

**Formula:**
```
=DGET(A1:D6, "Price", F1:F2)
```

**Result:** `#VALUE!`

**Explanation:** No products have Category "Outdoor". DGET returns #VALUE! when zero records match.

---

### Example 6: Three-Criteria Lookup

**Database (A1:E7):**
| Region | Year | Product | Sales | Units |
|--------|------|---------|-------|-------|
| East | 2023 | Widget | 50000 | 500 |
| East | 2024 | Widget | 55000 | 520 |
| West | 2023 | Widget | 45000 | 480 |
| West | 2024 | Widget | 48000 | 490 |
| East | 2024 | Gadget | 30000 | 300 |
| West | 2024 | Gadget | 28000 | 290 |

**Criteria (G1:I2):**
| Region | Year | Product |
|--------|------|---------|
| East | 2024 | Widget |

**Formula:**
```
=DGET(A1:E7, "Sales", G1:I2)
```

**Result:** `55000`

**Explanation:** Only one record matches all three criteria (Region=East, Year=2024, Product=Widget). Returns the Sales value of 55000.

---

### Example 7: Comparison Operator in Criteria

**Database (A1:D6):** Same as Example 1

**Criteria (F1:G2):**
| Category | Price |
|----------|-------|
| Home | >50 |

**Formula:**
```
=DGET(A1:D6, "Product", F1:G2)
```

**Result:** `Gadget Y`

**Explanation:** Only Gadget Y is both in the Home category AND has a price greater than 50.

---

### Example 8: Retrieving Text Values

**Database (A1:E6):**
| ID | Employee | Department | Status | Salary |
|----|----------|------------|--------|--------|
| 101 | Alice | Sales | Active | 55000 |
| 102 | Bob | Marketing | Active | 48000 |
| 103 | Carol | Sales | Leave | 62000 |
| 104 | David | IT | Active | 75000 |
| 105 | Eve | Sales | Active | 58000 |

**Criteria (G1:G2):**
| ID |
|----|
| 103 |

**Formula:**
```
=DGET(A1:E6, "Employee", G1:G2)
```

**Result:** `Carol`

**Explanation:** Employee ID 103 uniquely identifies Carol. DGET returns the text value from the Employee column.

---

### Example 9: Handling with IFERROR

**Database (A1:E6):** Same as Example 8

**Criteria (G1:G2):**
| Department |
|------------|
| Sales |

**Formula:**
```
=IFERROR(DGET(A1:E6, "Employee", G1:G2), "Multiple or no matches")
```

**Result:** `Multiple or no matches`

**Explanation:** Three Sales employees exist, causing #NUM!. IFERROR catches this and returns a friendly message.

---

### Example 10: Validating Data Uniqueness

**Database (A1:D8):**
| Product | SKU | Category | Price |
|---------|-----|----------|-------|
| Widget A | WA-001 | Electronics | 25 |
| Widget B | WB-001 | Electronics | 35 |
| Gadget X | GX-001 | Home | 45 |
| Gadget Y | GY-001 | Home | 55 |
| Widget A | WA-002 | Electronics | 28 |
| Device Z | DZ-001 | Electronics | 65 |
| Gadget X | GX-002 | Home | 42 |

**Criteria (F1:F2):**
| Product |
|---------|
| Widget A |

**Formula:**
```
=DGET(A1:D8, "SKU", F1:F2)
```

**Result:** `#NUM!`

**Explanation:** Two records have Product "Widget A" (with different SKUs). DGET errors, revealing duplicate product names in the database.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| #NUM! | Multiple records match the criteria | Add more specific criteria to isolate a single record; check for duplicate data |
| #VALUE! | No records match the criteria | Verify criteria values exist in the database; check for typos or formatting differences |
| #VALUE! | Field parameter is invalid | Ensure field name matches a column header exactly or column number is within range |
| #NAME? | Field name not enclosed in quotes | Use quotation marks: "Sales" not Sales |
| Wrong value | Criteria headers do not match database headers | Copy headers directly from database to criteria range |
| Wrong value | Extra criteria rows with spaces | Ensure criteria range includes only header and actual criteria rows |
| #REF! | Invalid range reference | Verify database and criteria ranges point to existing cells |

## Use Cases

### Configuration Value Lookup

**Scenario:** An application uses a configuration table in Excel where each setting has a unique key. DGET retrieves specific configuration values while validating that each key appears exactly once.

**Implementation:** Create a configuration database with Key, Value, Description, and Last Modified columns. Use DGET to retrieve values by key. If #NUM! occurs, it indicates a duplicate key that must be resolved.

**Technical Details:** The formula `=DGET(ConfigTable, "Value", LookupCriteria)` retrieves settings. Wrap in IFERROR for graceful handling: `=IFERROR(DGET(...), "CONFIG ERROR: Check for duplicates")`. This pattern ensures configuration integrity while providing clear error messages. Multiple formulas can reference the same criteria range, allowing dynamic lookups by changing a single cell.

---

### Invoice Line Item Retrieval

**Scenario:** A billing system needs to retrieve specific charges from an invoice detail table using a combination of Invoice Number and Line Item Number to ensure the exact charge is identified.

**Implementation:** Structure the invoice database with Invoice Number, Line Number, Description, Amount, and Tax columns. The criteria range includes both identifiers to uniquely locate each charge.

**Technical Details:** Use criteria like Invoice="INV-2024-001" AND Line=3 to retrieve specific amounts. The #NUM! error if triggered indicates duplicate line items (data entry error) or missing records (#VALUE! error). This validation is valuable for audit trails where retrieving the wrong amount would be problematic.

---

### Employee Lookup by Unique Identifier

**Scenario:** An HR system needs to retrieve employee details using their unique employee ID, with validation that IDs are truly unique across the organization.

**Implementation:** Build an employee database with ID, Name, Department, Title, Hire Date, and Salary columns. DGET retrieves any field based on the employee ID criteria.

**Technical Details:** The formula `=DGET(Employees, "Name", IDCriteria)` returns the employee name for a given ID. If someone accidentally creates a duplicate ID, DGET immediately errors rather than silently returning the wrong person's data. This makes DGET safer than VLOOKUP for scenarios where data integrity is critical.

## Platform Differences

| Feature | Excel | Google Sheets |
|---------|-------|---------------|
| Basic functionality | Fully supported | Fully supported |
| Error on multiple matches | Returns #NUM! | Returns #NUM! |
| Error on no matches | Returns #VALUE! | Returns #VALUE! |
| Case sensitivity | Case-insensitive | Case-insensitive |
| Wildcard support (*, ?) | Supported in criteria | Supported in criteria |
| Return types | Numbers, text, dates, errors | Numbers, text, dates, errors |
| Structured table references | Supported | Not supported |
| Named ranges | Fully supported | Fully supported |
| Maximum database rows | 1,048,576 | ~10,000,000 cells |
| Array formula usage | Returns single value | Returns single value |

**Key Difference:** Both platforms behave identically for DGET. The #NUM! error for multiple matches and #VALUE! for no matches are consistent across Excel and Google Sheets.

## Tips and Best Practices

1. **Design criteria for uniqueness**: Before using DGET, identify which column or column combination uniquely identifies records. Use those columns in your criteria to ensure exactly one match. Common unique identifiers include ID numbers, email addresses, or compound keys (Region + Year + Product).

2. **Always handle errors**: DGET is designed to error when matches are not exactly one. Use IFERROR to provide meaningful feedback: `=IFERROR(DGET(...), IF(DCOUNT(...)=0, "Not found", "Multiple matches"))`. This tells users whether to add criteria (multiple matches) or check spelling (not found).

3. **Use DGET for data validation**: The strict one-match requirement makes DGET valuable for validating data uniqueness. If `=DGET(database, "ID", criteria)` errors with #NUM!, you have duplicate records that need cleanup.

4. **Combine with DCOUNT for diagnostics**: Before DGET, use `=DCOUNT(database, field, criteria)` to see how many records match. This helps diagnose why DGET might fail: 0 means no matches, 1 means success, >1 means too many matches.

5. **Consider INDEX/MATCH for first-match scenarios**: If you do not care about uniqueness and just want the first matching value, use INDEX/MATCH or XLOOKUP instead. DGET is specifically for scenarios where multiple matches indicate a problem.

6. **Use wildcards carefully**: Wildcards like "*" can accidentally match multiple records. When using DGET, wildcards should only be used when you are certain the pattern matches exactly one record.

## Related Functions

### Similar Functions

| Function | Purpose | Key Difference |
|----------|---------|----------------|
| [[VLOOKUP]] | Looks up value by first column | Returns first match; does not validate uniqueness |
| [[XLOOKUP]] | Flexible lookup function | Returns first match; more options than VLOOKUP |
| [[INDEX]]/[[MATCH]] | Flexible lookup combination | Returns first match; no uniqueness validation |
| [[DSUM]] | Sums values matching criteria | Aggregates multiple matches instead of erroring |
| [[DAVERAGE]] | Averages values matching criteria | Aggregates multiple matches instead of erroring |
| [[FILTER]] | Returns all matching records | Returns array of all matches, not single value |

### Commonly Used With

**[[DCOUNT]]** - Verify match count before DGET:
```
=IF(DCOUNT(database, "ID", criteria)=1, DGET(database, "Value", criteria), "Check criteria")
```
Provides a custom message instead of #NUM! or #VALUE! errors.

**[[IFERROR]]** - Handle DGET errors gracefully:
```
=IFERROR(DGET(database, "Name", criteria), "Not found or duplicate")
```
Prevents error values from displaying in reports.

**[[ISERROR]]** - Validate data uniqueness:
```
=IF(ISERROR(DGET(database, "ID", criteria)), "Duplicate IDs exist", "IDs are unique")
```
Uses DGET's strict behavior as a validation tool.

## Official Documentation

- [Microsoft Excel DGET Documentation](https://support.microsoft.com/en-us/office/dget-function-455568bf-4eef-45f7-90f0-ec250d00892e)
- [Google Sheets DGET Documentation](https://support.google.com/docs/answer/3094144)

## Version History

| Version | Platform | Changes |
|---------|----------|---------|
| Excel 2.0 (1987) | Excel | Initial release as part of database functions |
| Excel 97 | Excel | Consistent error behavior standardized |
| Excel 2007 | Excel | Row limit increased to 1,048,576 |
| Excel 2010 | Excel | Performance optimizations |
| Excel 365 | Excel | Table reference support enhanced |
| Initial release | Google Sheets | Full compatibility with Excel behavior |
| 2018 | Google Sheets | Performance improvements |
