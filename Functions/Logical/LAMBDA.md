---
categories:
- spreadsheet-functions
subCategories:
- excel
- sheets
topics:
- logical
- advanced-formulas
subTopics:
- custom-functions
- reusable-formulas
- functional-programming
- named-functions
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Lambda Function
- Custom Function
- User Defined Function
- Named Function
- Anonymous Function
tags:
- lambda
- custom-functions
- advanced
- functional-programming
- reusable
- named-functions
---

# LAMBDA

## Description

**LAMBDA** revolutionizes spreadsheet formulas by enabling you to create custom, reusable functions without VBA, Apps Script, or any programming environment. You define parameters, write the calculation logic, and then call your custom function like any built-in function. This transforms complex, error-prone formulas into clean, maintainable, self-documenting functions that can be used throughout your workbook. LAMBDA is the closest spreadsheets come to true programming while remaining within the formula paradigm.

At its core, LAMBDA separates the definition of a calculation from its execution. Instead of writing `=(A1*B1)-(A1*B1*C1)` in every cell where you need this calculation, you define a LAMBDA like `=LAMBDA(price, qty, discount, (price*qty)-(price*qty*discount))` once, name it (e.g., "NetRevenue"), and then call `=NetRevenue(A1,B1,C1)` wherever needed. Changes to the logic only need to happen in one place. This is the fundamental principle of abstraction that makes large software projects manageable - now available in spreadsheets.

**Named Functions are the key:** A LAMBDA by itself is just an anonymous function definition. To make it reusable, you must assign it to a Name (Excel: Formulas > Name Manager; Sheets: Data > Named Functions). Once named, your custom function appears in autocomplete and can be called from any cell. This is where LAMBDA's true power emerges: your spreadsheets develop their own vocabulary of domain-specific functions that match your business logic exactly.

**Platform differences and helper functions:** Excel 365 introduced LAMBDA along with helper functions MAP, REDUCE, SCAN, BYROW, BYCOL, and MAKEARRAY that enable functional programming patterns. Google Sheets added LAMBDA later with similar capabilities. Both platforms now support recursive LAMBDAs (functions that call themselves) for advanced algorithms like tree traversal, factorial calculation, or iterative convergence. Understanding these helper functions unlocks LAMBDA's full potential for array manipulation and complex data transformations.

## Syntax

> [!f(x)] LAMBDA Syntax
>
> ```
> =LAMBDA([parameter1], [parameter2], ..., calculation)
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `parameter1, parameter2, ...` | No | Names for the inputs your function will accept. Up to 253 parameters allowed. These become variables used in the calculation. Parameter names must be valid (letters, numbers, underscores; cannot start with number; cannot be cell references like A1). If no parameters, LAMBDA creates a thunk (delayed evaluation). |
| `calculation` | Yes | The formula that computes the result. This expression uses the parameter names as variables. The calculation is the last argument and defines what the function returns. Can include any valid formula, other functions, and even calls to other named LAMBDAs. |

### Return Value

When a LAMBDA is called with arguments, it returns the result of the calculation expression with the parameter values substituted in. If called without sufficient arguments, returns a partial application (curried function) in some contexts. The return value can be any type: number, text, Boolean, error, array, or even another LAMBDA.

### Named Function Setup

**Excel:**
1. Formulas > Name Manager > New
2. Name: Your function name (e.g., "NETREVENUE")
3. Refers to: `=LAMBDA(price,qty,discount,(price*qty)-(price*qty*discount))`
4. OK, Close

**Google Sheets:**
1. Data > Named functions > Add new function
2. Function name: Your name (e.g., "NETREVENUE")
3. Argument placeholders: price, qty, discount
4. Formula definition: `(price*qty)-(price*qty*discount)`
5. Descriptions are optional but recommended
6. Save

## Examples

> [!f(x)] LAMBDA Examples

### Example 1: Basic Two-Parameter LAMBDA
```
=LAMBDA(x, y, x + y)(3, 5)
```
**Result:** 8

**Explanation:** This inline LAMBDA defines parameters x and y, with calculation x+y. The (3, 5) at the end immediately calls it with those values. While this inline usage demonstrates how LAMBDA works, real-world use involves naming the function for reuse.

---

### Example 2: Named Custom Function for Tax Calculation
```
Name: ADDTAX
Refers to: =LAMBDA(amount, rate, amount * (1 + rate))

Usage: =ADDTAX(100, 0.08)
```
**Result:** 108

**Explanation:** Create a named function ADDTAX that adds tax to any amount. Once defined, use it anywhere like a built-in function. The logic (multiplying by 1+rate) is encapsulated and doesn't need to be repeated or remembered.

---

### Example 3: String Manipulation LAMBDA
```
Name: PROPERCASE
Refers to: =LAMBDA(text, UPPER(LEFT(text,1)) & LOWER(MID(text,2,LEN(text)-1)))

Usage: =PROPERCASE("jOHN sMITH")
```
**Result:** "John smith" (first letter upper, rest lower)

**Explanation:** Custom text transformation function. Combines UPPER, LEFT, MID, LOWER, and LEN into one reusable function. Useful when PROPER function behavior doesn't match your needs exactly.

---

### Example 4: Commission Calculator with Tiered Rates
```
Name: COMMISSION
Refers to: =LAMBDA(sales, IF(sales>100000, sales*0.10, IF(sales>50000, sales*0.07, sales*0.05)))

Usage: =COMMISSION(B2)
```
**Result:** Appropriate commission based on sales tier

**Explanation:** Encapsulates complex tiered logic in a named function. Business rules are defined once and consistently applied everywhere. When commission rates change, update the LAMBDA definition, and all cells recalculate automatically.

---

### Example 5: LAMBDA with Array Return
```
Name: SPLITNAME
Refers to: =LAMBDA(fullname, LET(space, FIND(" ", fullname), HSTACK(LEFT(fullname, space-1), MID(fullname, space+1, 100))))

Usage: =SPLITNAME("John Smith")
```
**Result:** {"John", "Smith"} (spills to two cells)

**Explanation:** LAMBDA can return arrays. This function splits a full name into first and last, returning both in adjacent cells. HSTACK combines the two parts into a horizontal array. Demonstrates LAMBDA + LET + array functions together.

---

### Example 6: Recursive LAMBDA for Factorial
```
Name: FACTORIAL
Refers to: =LAMBDA(n, IF(n<=1, 1, n * FACTORIAL(n-1)))

Usage: =FACTORIAL(5)
```
**Result:** 120 (5*4*3*2*1)

**Explanation:** LAMBDA can call itself recursively. This factorial function demonstrates recursion: if n<=1, return 1; otherwise, return n times FACTORIAL of (n-1). Works because the named function can reference itself by name.

---

### Example 7: Using LAMBDA with MAP for Array Transformation
```
=MAP(A1:A10, LAMBDA(x, x * 1.1))
```
**Result:** Each value in A1:A10 multiplied by 1.1

**Explanation:** MAP applies a LAMBDA to each element of an array. Here, anonymous LAMBDA multiplies each value by 1.1 (10% increase). MAP + LAMBDA replaces many array formula patterns with cleaner, more readable code.

---

### Example 8: REDUCE for Running Total
```
=REDUCE(0, A1:A10, LAMBDA(acc, val, acc + val))
```
**Result:** Sum of A1:A10 (equivalent to SUM)

**Explanation:** REDUCE accumulates values across an array. Starting with 0 (initial accumulator), for each value, add it to the running total. While SUM is simpler for addition, REDUCE handles complex accumulation patterns SUM cannot.

---

### Example 9: BYROW for Row-Wise Calculations
```
=BYROW(A1:C10, LAMBDA(row, MAX(row) - MIN(row)))
```
**Result:** Range (max-min) for each row, returned as column

**Explanation:** BYROW applies LAMBDA to each row of a range. Here, it calculates the range (spread between max and min) for each row. Returns a single-column array with one result per input row.

---

### Example 10: BYCOL for Column Statistics
```
=BYCOL(A1:E10, LAMBDA(col, AVERAGE(col)))
```
**Result:** Average of each column, returned as row

**Explanation:** BYCOL applies LAMBDA to each column. Calculates column averages, returning a single-row array. Useful for summary statistics across multiple data columns in one formula.

---

### Example 11: SCAN for Running Calculations
```
=SCAN(0, A1:A10, LAMBDA(acc, x, acc + x))
```
**Result:** Running total showing cumulative sum at each step

**Explanation:** SCAN is like REDUCE but returns all intermediate values, not just the final result. Perfect for running totals, cumulative products, or any calculation where you need the progression, not just the endpoint.

---

### Example 12: MAKEARRAY for Dynamic Matrix Creation
```
=MAKEARRAY(5, 5, LAMBDA(r, c, r * c))
```
**Result:** 5x5 multiplication table

**Explanation:** MAKEARRAY generates an array by calling LAMBDA for each position. Parameters r (row) and c (column) are 1-indexed. This creates a multiplication table. Use for any computed matrix based on position.

---

### Example 13: Currying / Partial Application
```
Name: ADDPERCENT
Refers to: =LAMBDA(pct, LAMBDA(value, value * (1 + pct)))

Usage:
=ADDPERCENT(0.2)(100)  → 120
Named as MARKUP20: =ADDPERCENT(0.2)
Then: =MARKUP20(100)   → 120
```
**Result:** Specialized function with preset percentage

**Explanation:** LAMBDA can return another LAMBDA, enabling partial application. ADDPERCENT(0.2) returns a new function that adds 20% to any value. This "currying" pattern lets you create specialized versions of general functions.

---

### Example 14: Complex Business Logic - Shipping Cost
```
Name: SHIPPINGCOST
Refers to: =LAMBDA(weight, zone, express,
    LET(
        base, VLOOKUP(zone, ShippingRates, 2, 0),
        perLb, VLOOKUP(zone, ShippingRates, 3, 0),
        cost, base + (weight * perLb),
        IF(express, cost * 1.5, cost)
    )
)

Usage: =SHIPPINGCOST(5, "Zone2", TRUE)
```
**Result:** Shipping cost with express modifier

**Explanation:** Complex business logic encapsulated cleanly. Uses LET for intermediate calculations, VLOOKUP for rate table lookup, and conditional for express modifier. All complexity hidden behind a simple three-parameter function call.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#CALC!` | LAMBDA called without enough arguments | Provide all required parameter values when calling the function. Check that number of arguments matches number of parameters. |
| `#NAME?` | Named function not found | Verify the Name exists in Name Manager (Excel) or Named Functions (Sheets). Check spelling. Names are case-insensitive but must match. |
| `#VALUE!` | Parameter name conflicts with cell reference | Parameter names cannot look like cell references (A1, BC5, etc.). Use descriptive names like "amount", "rate", "inputValue". |
| `Circular reference` | Recursive LAMBDA without base case | Ensure recursive LAMBDAs have a terminating condition (e.g., IF(n<=1, 1, ...)) that stops recursion. |
| `#NUM!` error | Recursive depth exceeded | Excel limits recursion depth. Refactor very deep recursion into iterative approaches using REDUCE or SCAN. |
| `Parameter not recognized` | Typo in parameter name within calculation | Parameter names in the calculation must exactly match the parameter definitions. Check spelling and case. |

## Use Cases

### [[Reusable Business Calculation Library]]

**Scenario:** A financial modeling team needs consistent calculation methods across hundreds of workbook cells, with easy updates when business rules change.

**Implementation:**
```
Name: GROSSMARGIN
Refers to: =LAMBDA(revenue, cogs, (revenue - cogs) / revenue)

Name: NETPRICE
Refers to: =LAMBDA(list, disc1, disc2, list * (1-disc1) * (1-disc2))

Name: DAYSALES
Refers to: =LAMBDA(receivables, revenue, receivables / (revenue / 365))

Usage throughout workbook:
=GROSSMARGIN(B5, B6)
=NETPRICE(C2, D2, E2)
=DAYSALES(Balance!F10, Income!G15)
```

**Business Application:** Financial analysts work faster with domain-specific functions that match their vocabulary. "GROSSMARGIN" is more meaningful than remembering the formula. When accounting standards change (e.g., how revenue is calculated), update the LAMBDA definition once, and every usage updates automatically.

**Technical Details:** Create a library sheet or use Name Manager to organize your LAMBDAs. Document each function's purpose and parameters. Consider version tracking for critical business logic. LAMBDAs can call other LAMBDAs, enabling modular formula design.

---

### [[Data Transformation Pipeline]]

**Scenario:** ETL-style data transformation where raw imported data needs consistent cleaning and reformatting across multiple columns.

**Implementation:**
```
Name: CLEANPHONE
Refers to: =LAMBDA(phone,
    TEXT(--SUBSTITUTE(SUBSTITUTE(SUBSTITUTE(SUBSTITUTE(phone, "-", ""), "(", ""), ")", ""), " ", ""),
    "(###) ###-####"))

Name: STANDARDNAME
Refers to: =LAMBDA(name, PROPER(TRIM(CLEAN(name))))

Name: PARSEDATE
Refers to: =LAMBDA(datetext, DATEVALUE(SUBSTITUTE(datetext, "/", "-")))

Usage with MAP for bulk transformation:
=MAP(RawPhones, LAMBDA(p, CLEANPHONE(p)))
=MAP(RawNames, LAMBDA(n, STANDARDNAME(n)))
```

**Business Application:** Imported data from various sources arrives in inconsistent formats. Rather than writing complex formulas repeatedly for each column, create cleaning functions once. New data imports use the same transformations with guaranteed consistency.

**Technical Details:** Chain multiple transformations by calling one LAMBDA from another. Use MAP to apply transformations across entire columns. Build a library of common data cleaning patterns (phone formatting, date parsing, address standardization) that your organization reuses.

---

### [[Recursive Hierarchical Calculations]]

**Scenario:** Calculate values through an organizational hierarchy, such as rolling up budget totals or counting employees under each manager.

**Implementation:**
```
Name: SUBORDINATECOUNT
Refers to: =LAMBDA(manager_id,
    LET(
        direct, FILTER(OrgData[EmployeeID], OrgData[ManagerID]=manager_id),
        direct_count, IF(ISERROR(direct), 0, ROWS(direct)),
        indirect_count, IF(direct_count=0, 0, SUM(MAP(direct, LAMBDA(e, SUBORDINATECOUNT(e))))),
        direct_count + indirect_count
    )
)

Usage: =SUBORDINATECOUNT(A2)
```

**Business Application:** Organizational hierarchies are inherently recursive. Traditional spreadsheet approaches require multiple helper columns and manual iteration. A recursive LAMBDA handles arbitrary depth automatically. Total headcount, budget rollups, and org structure analysis become simple function calls.

**Technical Details:** Recursive LAMBDAs must have a base case (here: when there are no direct reports, return 0). Watch for maximum recursion depth limits. For very deep hierarchies, consider iterative approaches or database solutions. Test with sample data before applying to full org charts.

---

### [[Dynamic KPI Calculations]]

**Scenario:** Dashboard KPIs that require complex, multi-step calculations that should be consistent and maintainable across the workbook.

**Implementation:**
```
Name: CUSTOMERLTV
Refers to: =LAMBDA(customer_id,
    LET(
        orders, FILTER(OrderData, OrderData[CustomerID]=customer_id),
        total_revenue, SUMPRODUCT((OrderData[CustomerID]=customer_id)*OrderData[Amount]),
        first_order, MIN(IF(OrderData[CustomerID]=customer_id, OrderData[Date])),
        months_active, (TODAY()-first_order)/30.44,
        monthly_value, total_revenue / MAX(months_active, 1),
        projected_ltv, monthly_value * 36,
        projected_ltv
    )
)

Usage: =CUSTOMERLTV(D5)
```

**Business Application:** Complex KPIs like Customer Lifetime Value involve multiple intermediate calculations. Encapsulating in LAMBDA makes the dashboard formula simple (just the customer ID) while the complexity lives in the definition. Analysts can trust the calculation without understanding every step.

**Technical Details:** Use LET within LAMBDA for readable intermediate values. Document assumptions (like the 36-month projection window). Consider breaking very complex LAMBDAs into smaller, composable functions (e.g., separate MONTHLYVALUE and PROJECTIONMULTIPLIER LAMBDAs).

## Platform Differences

### Microsoft Excel

- **Availability:** Excel 365 (subscription), Excel 2024 (perpetual)
- **NOT available:** Excel 2021, Excel 2019, or earlier
- **Named functions:** Defined via Formulas > Name Manager
- **Helper functions:** MAP, REDUCE, SCAN, BYROW, BYCOL, MAKEARRAY all available
- **Recursion limit:** Practical limit around 10,000 levels depending on complexity
- **LET integration:** LAMBDA works seamlessly with LET for readable complex formulas

### Google Sheets

- **Availability:** All current versions (added 2022)
- **Named functions:** Defined via Data > Named functions (with dedicated UI)
- **Helper functions:** MAP, REDUCE, SCAN, BYROW, BYCOL, MAKEARRAY all available
- **Recursion limit:** Similar practical limits; deep recursion may hit computation limits
- **UI advantages:** Sheets provides a dedicated named function interface with descriptions and argument documentation
- **LAMBDA function:** Also available as standalone function like Excel

### Key Difference Alert

The main differences are in **how named functions are created and managed**:
- **Excel** uses Name Manager (the same interface used for named ranges), which can feel less intuitive for LAMBDA specifically
- **Google Sheets** has a dedicated Named Functions interface that feels more like defining custom functions, with fields for descriptions and argument help

For cross-platform compatibility, stick to basic LAMBDA patterns. Both platforms support the helper functions (MAP, REDUCE, etc.), but test your specific use cases when sharing workbooks between platforms.

## Tips and Best Practices

1. **Name your LAMBDAs:** Inline LAMBDAs (called immediately) are rarely useful. The power comes from naming them for reuse. Always create a Name for any LAMBDA you'll use more than once.

2. **Use descriptive parameter names:** Instead of `LAMBDA(x, y, x*y)`, use `LAMBDA(price, quantity, price*quantity)`. Self-documenting parameters make formulas readable months later.

3. **Combine with LET:** Complex LAMBDAs benefit from LET for intermediate calculations:
   ```
   =LAMBDA(x, LET(step1, x*2, step2, step1+10, step2/5))
   ```
   This is far more readable than nested calculations.

4. **Build libraries of functions:** Create organizational standards for common calculations. Share LAMBDAs across teams via template workbooks. Document your function library.

5. **Test recursion carefully:** Recursive LAMBDAs are powerful but can hit stack limits or cause performance issues. Always include a base case and test with edge cases (empty input, single item, very large datasets).

6. **Start simple, refactor later:** Don't try to write complex LAMBDAs from scratch. Write the formula normally first, verify it works, then wrap it in LAMBDA for reuse.

7. **Use MAP/REDUCE for arrays:** Instead of array formulas or helper columns, use MAP to transform arrays and REDUCE to accumulate values. These patterns are more readable and maintainable.

8. **Document your functions:** In Google Sheets, use the description fields. In Excel, add a comment in the Name or create a documentation sheet. Future users (including yourself) will thank you.

9. **Watch for parameter shadowing:** If you nest LAMBDAs, inner parameter names can shadow outer ones. Use distinct names across nested functions.

10. **Consider performance:** Very complex LAMBDAs or deep recursion can slow calculations. For performance-critical workbooks, profile and optimize hot paths.

## Related Functions

### Similar Functions
| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[LET]] | Defines named values within a formula | For intermediate calculations that don't need to be reusable across cells |
| [[Named Ranges]] | Defines reusable cell references | For static data references, not calculations |
| [[Custom VBA/Apps Script]] | Full programming capability | When LAMBDA's limitations (no loops, limited external access) are blocking |

### Commonly Used Together

**[[MAP]]** - Applies LAMBDA to each element of an array

*Transforming each value in a range:*
```
=MAP(Prices, LAMBDA(p, p * 1.15))
```
MAP replaces array formulas for element-wise transformations. The LAMBDA defines what happens to each element.

---

**[[REDUCE]]** - Accumulates array elements into single value

*Custom aggregation with LAMBDA:*
```
=REDUCE(1, A1:A10, LAMBDA(product, val, product * val))
```
Computes the product of all values (no built-in PRODUCT equivalent for custom operations). REDUCE calls LAMBDA for each element, carrying the accumulated result.

---

**[[SCAN]]** - Like REDUCE but returns all intermediate values

*Running product or cumulative calculation:*
```
=SCAN(0, Sales, LAMBDA(running, sale, running + sale))
```
Returns an array showing the running total at each position, not just the final sum.

---

**[[BYROW]] / [[BYCOL]]** - Apply LAMBDA to each row or column

*Row-wise custom calculation:*
```
=BYROW(DataRange, LAMBDA(row, STDEV(row)))
```
Calculates standard deviation for each row, returning a column of results. BYCOL does the same for columns.

---

**[[MAKEARRAY]]** - Generate array using LAMBDA for each position

*Creating computed matrices:*
```
=MAKEARRAY(10, 10, LAMBDA(r, c, IF(r=c, 1, 0)))
```
Creates a 10x10 identity matrix. LAMBDA receives row and column indices, returns the cell value.

---

**[[LET]]** - Named intermediate calculations

*Combining LET with LAMBDA for readability:*
```
=LAMBDA(input, LET(clean, TRIM(input), upper, UPPER(clean), upper))
```
LET inside LAMBDA creates named steps, making complex transformations readable.

## Official Documentation

- **Microsoft Excel:** [LAMBDA function](https://support.microsoft.com/en-us/office/lambda-function-bd212d27-1cd1-4321-a34a-ccbf254b8b67)
- **Microsoft Excel:** [Create named LAMBDA functions](https://support.microsoft.com/en-us/office/create-custom-functions-in-excel-a0c78294-08cb-46b8-88cc-f7f8502cbf5e)
- **Google Sheets:** [LAMBDA function](https://support.google.com/docs/answer/12508718)
- **Google Sheets:** [Named functions](https://support.google.com/docs/answer/12504534)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel 365 | December 2020 | Initial LAMBDA release |
| Excel 365 | 2021 | MAP, REDUCE, SCAN, BYROW, BYCOL, MAKEARRAY added |
| Excel 2024 | 2024 | Included in perpetual license |
| Google Sheets | August 2022 | LAMBDA and named functions added |
| Google Sheets | 2022 | MAP, REDUCE, SCAN, BYROW, BYCOL, MAKEARRAY added |

---

*Last updated: 2026-01-10*
