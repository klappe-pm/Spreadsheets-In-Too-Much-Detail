---
categories:
  - spreadsheet-functions
subCategories:
  - excel
  - sheets
topics:
  - information
subTopics:
  - na-detection
  - lookup-validation
  - not-available
  - missing-data
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
  - is-na
  - na-check
  - not-available-test
  - lookup-error-check
tags:
  - information
  - error-handling
  - na-detection
  - lookup-functions
  - data-validation
  - excel
  - google-sheets
---

# ISNA

## Description

The **ISNA function** tests whether a value is the #N/A error specifically and returns TRUE only for #N/A errors, returning FALSE for all other values including other error types. The #N/A error (meaning "Not Available" or "No Answer") is unique among Excel/Sheets errors because it typically represents a legitimate data condition rather than a formula mistake—specifically, it indicates that a lookup function could not find the requested value in the search range. ISNA is the precision tool for detecting this specific condition, unlike ISERROR which catches all errors indiscriminately. This specificity makes ISNA the preferred choice for handling lookup function results.

The primary use case for ISNA is handling lookup function outcomes. When VLOOKUP, HLOOKUP, MATCH, INDEX+MATCH, or XLOOKUP cannot find the lookup value, they return #N/A. This isn't necessarily a problem—it's often expected when looking up values that might not exist in the data. For example, searching a customer database for a new customer appropriately returns #N/A because that customer hasn't been added yet. Using ISNA allows you to detect this specific "not found" condition and handle it appropriately (display "New Customer", show a default value, or trigger a different workflow) while still allowing other errors (#REF!, #VALUE!) to surface as warnings of actual formula problems. The pattern =IF(ISNA(lookup), "Not found", lookup) is ubiquitous in spreadsheet work.

The critical advantage of ISNA over ISERROR is error preservation for debugging. Consider a VLOOKUP formula where someone accidentally deletes the lookup column: ISERROR would catch the resulting #REF! and hide it behind your alternative value, making the broken formula invisible. ISNA only catches #N/A, so the #REF! error would still display, alerting you to the problem. This distinction is crucial in production spreadsheets where formula integrity matters. Modern Excel offers IFNA as a streamlined alternative to IF+ISNA, and XLOOKUP includes a built-in "if not found" parameter, but ISNA remains valuable for conditional logic, counting NA values, and compatibility with older systems.

Platform behavior for ISNA is identical between Excel and Google Sheets. Both return TRUE exclusively for #N/A errors and FALSE for everything else, including other errors. The function has been available since early versions of both platforms. IFNA (the companion function that returns an alternative value if #N/A) was added to Excel in 2013 and has always been available in Google Sheets. In array contexts, ISNA works with ARRAYFORMULA in Google Sheets and with dynamic arrays in Excel 365. The function is completely portable across platforms and versions.

## Syntax

> [!f(x)] ISNA Syntax
>
> ```
> =ISNA(value)
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `value` | Yes | The value, cell reference, or formula result to test for #N/A. Can be any Excel/Sheets expression. Returns TRUE only if the value is the #N/A error. Returns FALSE for all other values including numbers, text, blanks, logical values, and all other error types (#VALUE!, #REF!, #DIV/0!, etc.). |

### Return Value

Returns **TRUE** if and only if the value is the #N/A error. Returns **FALSE** for any other value, including all other error types. ISNA never produces an error—it always returns TRUE or FALSE.

## Examples

> [!f(x)] ISNA Examples

### Example 1: Testing #N/A Directly
```
=ISNA(#N/A)
```
**Result:** TRUE

**Explanation:** The #N/A error value returns TRUE. This is the only value for which ISNA returns TRUE.

---

### Example 2: Testing Other Error Types
```
=ISNA(#DIV/0!)
=ISNA(#VALUE!)
=ISNA(#REF!)
```
**Result:** FALSE (all three)

**Explanation:** ISNA only detects #N/A. All other error types return FALSE. Use ISERROR to catch all errors, or ISERR to catch all except #N/A.

---

### Example 3: Testing Valid Values
```
=ISNA(100)
=ISNA("Hello")
=ISNA(TRUE)
```
**Result:** FALSE (all three)

**Explanation:** Non-error values always return FALSE. Numbers, text, and logical values are not #N/A errors.

---

### Example 4: VLOOKUP with ISNA Error Handling
```
=IF(ISNA(VLOOKUP(A1, Products, 2, FALSE)), "Product not found", VLOOKUP(A1, Products, 2, FALSE))
```
**Scenario:** Look up product name, showing message if not found
**Result:** Returns product name or "Product not found"

**Explanation:** This is the classic ISNA pattern. If the product ID isn't in the list, VLOOKUP returns #N/A, ISNA catches it, and the alternative message displays. Other errors (like #REF! from deleted columns) still show, alerting you to formula problems.

---

### Example 5: Using IFNA (Modern Alternative)
```
=IFNA(VLOOKUP(A1, Products, 2, FALSE), "Product not found")
```
**Scenario:** Same as above, but cleaner syntax
**Result:** Returns product name or "Product not found"

**Explanation:** IFNA is the modern equivalent of IF+ISNA. It's cleaner, evaluates the formula only once, and is the recommended approach in Excel 2013+ and all Google Sheets versions.

---

### Example 6: INDEX+MATCH with ISNA
```
=IF(ISNA(MATCH(A1, B:B, 0)), "No match", INDEX(C:C, MATCH(A1, B:B, 0)))
```
**Scenario:** Find value in column C based on lookup in column B
**Result:** Returns matched value or "No match"

**Explanation:** MATCH returns #N/A when it can't find the lookup value. ISNA detects this before the INDEX function runs, providing a clean alternative.

---

### Example 7: Counting #N/A Values in a Range
```
=SUMPRODUCT(ISNA(A1:A100)*1)
```
**Scenario:** Count how many lookup results returned "not found"
**Result:** Number of #N/A values in the range

**Explanation:** ISNA evaluates each cell, returning TRUE for #N/A. Multiplying by 1 converts to 1/0, and SUMPRODUCT sums them. Useful for data quality metrics.

---

### Example 8: Conditional Formatting for #N/A
```
Conditional Formatting Formula: =ISNA(A1)
Format: Yellow background
```
**Scenario:** Highlight cells where lookups failed to find matches
**Result:** #N/A cells highlighted, other errors not highlighted

**Explanation:** Using ISNA in conditional formatting creates visual distinction between "not found" (possibly expected) and other errors (likely problems).

---

### Example 9: Chained Lookup with ISNA
```
=IF(ISNA(VLOOKUP(A1, Table1, 2, FALSE)),
    IF(ISNA(VLOOKUP(A1, Table2, 2, FALSE)),
        "Not in any table",
        VLOOKUP(A1, Table2, 2, FALSE)),
    VLOOKUP(A1, Table1, 2, FALSE))
```
**Scenario:** Try primary table, then fall back to secondary table
**Result:** Value from Table1, or Table2, or message if neither has it

**Explanation:** ISNA enables fallback chains. First lookup is tried; if #N/A, second is tried; if both fail, show message. Other errors bubble up.

---

### Example 10: Distinguishing #N/A from Other Errors
```
=IF(ISNA(A1), "Not Found", IF(ISERR(A1), "Formula Error", A1))
```
**Scenario:** Provide different messages for different error types
**Result:** "Not Found" for #N/A, "Formula Error" for other errors, or the value

**Explanation:** ISNA first checks for the expected "not found" case. ISERR catches remaining errors. Valid values pass through. This pattern gives maximum user feedback.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `ISNA returns FALSE for other errors` | ISNA only detects #N/A | This is correct behavior; use ISERROR to catch all errors |
| `Unexpected #REF! showing through` | ISNA doesn't catch #REF! | This is intentional—it helps identify broken formulas; wrap in ISERROR only if you want to hide all errors |
| `Double evaluation performance` | IF+ISNA evaluates lookup twice | Use IFNA instead: =IFNA(lookup, alternative) |
| `ISNA not catching text "#N/A"` | Text that looks like #N/A isn't an error | The text string "#N/A" is not an error value; use =A1="#N/A" to test for the text |
| `Formula shows #N/A when ISNA should catch it` | ISNA test might be on wrong cell | Verify the cell reference; ISNA must test the cell containing the error |
| `IFNA not available` | Excel version before 2013 | Use the IF+ISNA pattern instead, or upgrade Excel |
| `Array formula not working` | Older Excel requires CSE | Use Ctrl+Shift+Enter for array formulas in pre-365 Excel |

## Use Cases

### [[Customer Lookup Validation System]]

**Scenario:** A sales order entry system uses VLOOKUP to retrieve customer information from a master customer database. When sales representatives enter customer IDs, the system needs to distinguish between "customer doesn't exist" (create new customer) and "system error" (contact IT). The behavior must be different for each scenario.

**Implementation:**
```
Customer Name Lookup:
=IF(ISNA(VLOOKUP(CustomerID, CustomerDB, 2, FALSE)),
    "NEW CUSTOMER - Please complete registration",
    VLOOKUP(CustomerID, CustomerDB, 2, FALSE))

Credit Limit Lookup (with error distinction):
=IF(ISNA(VLOOKUP(CustomerID, CustomerDB, 5, FALSE)),
    0,
    IF(ISERR(VLOOKUP(CustomerID, CustomerDB, 5, FALSE)),
        "DB ERROR - Contact IT",
        VLOOKUP(CustomerID, CustomerDB, 5, FALSE)))

Customer Status Dashboard:
New Customers Entered Today: =SUMPRODUCT(ISNA(TodaysLookups)*1)
Database Errors: =SUMPRODUCT(ISERR(TodaysLookups)*1)
Successful Lookups: =COUNTA(TodaysLookups)-SUMPRODUCT(ISERROR(TodaysLookups)*1)

Order Validation:
=IF(ISNA(VLOOKUP(CustomerID, CustomerDB, 1, FALSE)),
    "HOLD: Customer not in system - Supervisor approval required",
    "OK: Customer verified")
```

**Business Application:** The sales process requires different workflows for new versus existing customers. New customers need onboarding forms, credit applications, and shipping address collection. A #N/A from the lookup correctly triggers this workflow. However, if the database connection is broken (#REF!) or someone entered text in a numeric ID field (#VALUE!), those are IT issues requiring different response. ISNA enables this critical distinction.

**Technical Details:** The credit limit lookup assigns 0 to new customers (safe default for orders) while surfacing database errors to IT. The dashboard provides management visibility into order processing—high "new customer" rates might indicate marketing success, while database errors need immediate IT attention. Consider caching successful lookups to reduce database hits on subsequent references.

---

### [[Data Matching and Reconciliation]]

**Scenario:** A finance team reconciles two data sources: internal accounting records and external bank statements. They use MATCH to find corresponding transactions. Some transactions legitimately don't match (timing differences, bank fees), but formula errors indicate serious problems with the reconciliation spreadsheet that need immediate attention.

**Implementation:**
```
Match Status:
=IF(ISNA(MATCH(AccountingRef, BankRefs, 0)),
    "UNMATCHED",
    "MATCHED")

Reconciliation Classification:
=IF(ISNA(MATCH(AccountingRef, BankRefs, 0)),
    IF(Amount<0, "UNMATCHED DEBIT", "UNMATCHED CREDIT"),
    "RECONCILED")

Reconciliation Summary:
Matched Items: =COUNTIF(StatusColumn, "MATCHED")
Unmatched Items: =SUMPRODUCT(ISNA(AllMatches)*1)
Formula Errors: =SUMPRODUCT(ISERR(AllMatches)*1)
Match Rate: =TEXT(COUNTIF(StatusColumn,"MATCHED")/COUNTA(AccountingRefs), "0.0%")

Integrity Check:
=IF(SUMPRODUCT(ISERR(AllMatches)*1)>0,
    "RECONCILIATION HALTED: " & SUMPRODUCT(ISERR(AllMatches)*1) & " formula errors detected",
    IF(SUMPRODUCT(ISNA(AllMatches)*1)>0,
        SUMPRODUCT(ISNA(AllMatches)*1) & " items require manual review",
        "All items reconciled"))
```

**Business Application:** Bank reconciliation is a critical financial control. Unmatched items (#N/A from MATCH) are normal and expected—they represent timing differences, outstanding checks, or items needing investigation. But formula errors indicate the reconciliation itself is broken, which could lead to missed discrepancies and audit failures. The integrity check ensures the reconciliation tool is working before analysts spend time on individual items.

**Technical Details:** Use ISNA with MATCH because MATCH returns #N/A for "not found." The classification formula adds context to unmatched items, helping prioritize review. Track match rates over time—declining rates might indicate data quality issues in either source system. For high-volume reconciliation, consider creating separate sheets for matched (archive) and unmatched (working) items.

---

### [[Product Catalog Cross-Reference]]

**Scenario:** A retailer maintains products in both an internal inventory system and an e-commerce platform. Products need SKU mapping between systems. New products legitimately have #N/A (not yet mapped), but other errors indicate integration problems. The catalog manager needs clear visibility into mapping status.

**Implementation:**
```
SKU Mapping Status:
=IF(ISNA(VLOOKUP(InternalSKU, MappingTable, 2, FALSE)),
    "PENDING MAPPING",
    IF(ISERR(VLOOKUP(InternalSKU, MappingTable, 2, FALSE)),
        "MAPPING ERROR",
        VLOOKUP(InternalSKU, MappingTable, 2, FALSE)))

Mapping Dashboard:
Total Products: =COUNTA(InternalSKUs)
Mapped: =COUNTA(InternalSKUs)-SUMPRODUCT(ISNA(Mappings)*1)-SUMPRODUCT(ISERR(Mappings)*1)
Pending Mapping: =SUMPRODUCT(ISNA(Mappings)*1)
Errors: =SUMPRODUCT(ISERR(Mappings)*1)
Mapping Completion: =TEXT((COUNTA(InternalSKUs)-SUMPRODUCT(ISNA(Mappings)*1))/COUNTA(InternalSKUs), "0.0%")

E-commerce Sync Readiness:
=IF(SUMPRODUCT(ISERR(Mappings)*1)>0,
    "BLOCKED: Fix " & SUMPRODUCT(ISERR(Mappings)*1) & " mapping errors first",
    IF(SUMPRODUCT(ISNA(Mappings)*1)>0,
        "WARNING: " & SUMPRODUCT(ISNA(Mappings)*1) & " products will be excluded from sync",
        "READY: All products mapped"))

Priority Mapping List (products with #N/A):
=FILTER(InternalSKUs, ISNA(Mappings))
```

**Business Application:** E-commerce integration requires accurate SKU mapping. Products with #N/A mapping are simply pending setup—not urgent unless they're about to launch. Mapping errors (other error types) indicate configuration problems that could cause sync failures. The dashboard gives the catalog manager clear action items: "Fix 3 errors" (urgent) versus "Map 47 new products" (scheduled work).

**Technical Details:** The FILTER formula creates a dynamic list of products needing mapping—useful for assigning work to team members. The sync readiness check enforces error resolution before allowing data sync, preventing bad data from reaching the e-commerce platform. Consider adding timestamp tracking for when products move from "pending" to "mapped" to measure team productivity.

## Platform Differences

### Microsoft Excel

- **Availability:** All versions (Excel 97 and later)
- **IFNA function:** Excel 2013 and later
- **Array behavior:** Native dynamic arrays in Excel 365; CSE required in older versions
- **Performance:** Highly optimized
- **XLOOKUP:** Excel 365 includes built-in "if not found" parameter, reducing need for ISNA

### Google Sheets

- **Availability:** All versions since launch
- **IFNA function:** All versions
- **Array behavior:** Requires ARRAYFORMULA wrapper for array operations
- **Performance:** Efficient for single cells and arrays
- **Query integration:** ISNA can be used in QUERY WHERE clauses for filtering

### Key Differences

| Feature | Excel | Google Sheets |
|---------|-------|---------------|
| ISNA availability | All versions | All versions |
| IFNA availability | 2013+ | All versions |
| Array syntax | Native in 365 | Requires ARRAYFORMULA |
| XLOOKUP alternative | Yes (365) | No XLOOKUP |
| Performance | Excellent | Excellent |
| In conditional formatting | Yes | Yes |

## Tips and Best Practices

1. **Prefer IFNA over IF+ISNA:** If you simply need to replace #N/A with an alternative value, =IFNA(lookup, "Not found") is cleaner and more efficient than =IF(ISNA(lookup), "Not found", lookup). IFNA evaluates the lookup only once.

2. **Use ISNA specifically for lookups:** ISNA is designed for detecting lookup "not found" conditions. For division errors, use IFERROR. For comprehensive error handling, use ISERROR. Match the tool to the error type.

3. **Don't use ISERROR for lookups:** ISERROR catches ALL errors, hiding formula problems like #REF! (deleted columns) or #NAME? (typos). ISNA lets those real problems surface while handling the expected "not found" case.

4. **Count with SUMPRODUCT:** =SUMPRODUCT(ISNA(range)*1) efficiently counts #N/A values in a range. This is useful for data quality dashboards and matching metrics.

5. **Layer ISNA with ISERR for complete handling:** The pattern =IF(ISNA(x), "Not found", IF(ISERR(x), "Error", x)) provides three-way handling: not found, other errors, and valid values.

6. **Consider XLOOKUP in Excel 365:** XLOOKUP has a built-in "if not found" parameter: =XLOOKUP(lookup, range, return, "Not found"). This eliminates need for ISNA wrapper in many cases.

## Related Functions

### Similar Functions

| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[IFNA]] | Returns alternative value if #N/A | For simple #N/A replacement without IF (Excel 2013+) |
| [[ISERROR]] | Tests for ANY error type | When you want to catch all errors including #N/A |
| [[ISERR]] | Tests for errors EXCEPT #N/A | When you want to catch errors but let #N/A through |
| [[ERROR.TYPE]] | Returns error type number | When you need to identify specific error types |
| [[IFERROR]] | Returns alternative for any error | When all errors should have same alternative (less precise) |

### Commonly Used Together

**[[IF]]** - Conditional logic based on #N/A status

*Classic ISNA error handling:*
```
=IF(ISNA(VLOOKUP(A1, B:C, 2, FALSE)), "Not found", VLOOKUP(A1, B:C, 2, FALSE))
```
The traditional pattern for handling lookup "not found" conditions.

---

**[[VLOOKUP]]** - Lookup function that returns #N/A when not found

*VLOOKUP with ISNA protection:*
```
=IFNA(VLOOKUP(A1, Data, 2, FALSE), 0)
```
Returns 0 instead of #N/A when the lookup value isn't found.

---

**[[MATCH]]** - Returns #N/A when value not found

*MATCH with ISNA handling:*
```
=IF(ISNA(MATCH(A1, B:B, 0)), "No match", MATCH(A1, B:B, 0))
```
MATCH returns #N/A for unfound values; ISNA catches this specific case.

---

**[[SUMPRODUCT]]** - Count #N/A values

*Count #N/A errors in range:*
```
=SUMPRODUCT(ISNA(A1:A100)*1)
```
Counts specifically #N/A values while ignoring other errors.

---

**[[ISERR]]** - Complement for non-NA errors

*Complete error categorization:*
```
=IF(ISNA(A1), "Not Found", IF(ISERR(A1), "Error", A1))
```
ISNA handles #N/A, ISERR handles other errors, valid values pass through.

## Official Documentation

- **Microsoft Excel:** [ISNA function](https://support.microsoft.com/en-us/office/is-functions-0f2d7971-6019-40a0-a171-f2d869135665)
- **Google Sheets:** [ISNA function](https://support.google.com/docs/answer/3093293)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel 97 | 1997 | Initial release |
| Excel 2003 | 2003 | No changes |
| Excel 2007 | 2007 | No changes |
| Excel 2010 | 2010 | No changes |
| Excel 2013 | 2013 | IFNA function added as complement |
| Excel 2016 | 2016 | No changes |
| Excel 2019 | 2019 | No changes |
| Excel 365 | 2018+ | XLOOKUP reduces need for ISNA |
| Excel for Mac | 2001+ | Full support |
| Excel Online | 2010+ | Full support |
| Google Sheets | 2006 | Available since launch |
| LibreOffice Calc | 1.0 | Full compatibility |
| Apple Numbers | 2007 | Full support |

---

*Last updated: 2026-01-10*
