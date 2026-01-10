---
categories:
- spreadsheet-functions
subCategories:
- excel
- sheets
topics:
- programming
- custom-functions
subTopics:
- user-defined-functions
- functional-programming
- reusable-formulas
- named-functions
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Lambda Function
- Custom Function
- User Defined Function
- Named Formula
- Reusable Function
tags:
- lambda
- custom-functions
- programming
- functional
- excel-365
- advanced
- udf
---

# LAMBDA

## Description

**LAMBDA** revolutionizes Excel by allowing you to create custom, reusable functions without VBA or add-ins. It transforms any formula into a named function that works just like built-in Excel functions. You define parameters, write the calculation logic, and save it to the Name Manager. From that point forward, you call your custom function by name, passing arguments just like SUM or VLOOKUP. This brings functional programming concepts directly into the spreadsheet environment.

The power of LAMBDA lies in abstraction and reusability. Instead of copying complex formulas across hundreds of cells and updating each one when business logic changes, you define the logic once as a LAMBDA, name it, and reference it everywhere. Change the LAMBDA definition, and every cell using it updates automatically. This dramatically improves formula maintainability, reduces errors, and makes spreadsheets more readable. A formula like `=CALCULATE_TAX(B5, C5, "2024")` is infinitely more understandable than a 200-character nested formula.

**LAMBDA in Google Sheets:** Google Sheets added LAMBDA support as "Named Functions" in 2022. While the syntax differs slightly in how you create and save them (Sheets uses Data > Named functions menu instead of Name Manager), the core LAMBDA functionality is identical. Formulas using inline LAMBDA work identically across platforms. This cross-platform support means you can develop sophisticated custom function libraries that work in both environments.

**Advanced LAMBDA patterns:** LAMBDA enables recursive functions, which was previously impossible in Excel formulas. You can create functions that call themselves to process nested data, traverse hierarchies, or perform iterative calculations. Combined with helper functions like MAP, REDUCE, SCAN, BYROW, and BYCOL, LAMBDA forms the foundation of a complete functional programming system within Excel. These patterns enable solutions that previously required VBA or external tools.

## Syntax

> [!f(x)] LAMBDA Syntax
>
> ```
> =LAMBDA([parameter1, parameter2, ...], calculation)
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `parameter1, parameter2, ...` | No | One or more names that represent values passed to the function. Maximum 253 parameters allowed. Parameters are local to the LAMBDA and can be any valid name (letters, numbers, underscores, but must start with letter). |
| `calculation` | Yes | The formula to execute, using the parameter names. This is the return value of the LAMBDA. Can reference other functions, including other LAMBDAs and even itself (recursion). |

### Return Value

Returns the result of the calculation expression using the values passed as arguments. The return type depends on the calculation: can be a number, text, logical value, error, or array. When used inline (not named), LAMBDA returns a LAMBDA function object that must be called or used with helper functions.

## Examples

> [!f(x)] LAMBDA Examples

### Example 1: Basic Inline LAMBDA with Immediate Call
```
=LAMBDA(x, x^2)(5)
```
**Result:** 25

**Explanation:** This LAMBDA takes one parameter `x` and returns x squared. The `(5)` at the end immediately calls the LAMBDA with argument 5. While useful for understanding LAMBDA, you'd typically name it rather than use inline like this.

---

### Example 2: Multi-Parameter LAMBDA
```
=LAMBDA(length, width, height, length*width*height)(10, 5, 2)
```
**Result:** 100

**Explanation:** Three parameters calculate volume. The order of arguments matches parameter order. When named (e.g., VOLUME), you'd call it as `=VOLUME(10, 5, 2)`.

---

### Example 3: Named LAMBDA - Percentage Calculation
Define in Name Manager:
- Name: `PCT_CHANGE`
- Refers to: `=LAMBDA(old, new, (new-old)/old)`

Usage in cells:
```
=PCT_CHANGE(100, 125)
```
**Result:** 0.25 (25% increase)

**Explanation:** Once named, the LAMBDA becomes a custom function. Call it like any Excel function. The Name Manager stores the definition; cells contain only the friendly call.

---

### Example 4: LAMBDA with Conditional Logic
Name: `GRADE`
```
=LAMBDA(score, IFS(score>=90,"A", score>=80,"B", score>=70,"C", score>=60,"D", TRUE,"F"))
```

Usage:
```
=GRADE(85)
```
**Result:** "B"

**Explanation:** LAMBDA can contain any formula complexity. This grading function encapsulates business logic that would otherwise be repeated across many cells.

---

### Example 5: LAMBDA Calling Another LAMBDA
Name: `CELSIUS`
```
=LAMBDA(fahrenheit, (fahrenheit-32)*5/9)
```

Name: `KELVIN`
```
=LAMBDA(fahrenheit, CELSIUS(fahrenheit)+273.15)
```

Usage:
```
=KELVIN(212)
```
**Result:** 373.15

**Explanation:** LAMBDAs can reference other named LAMBDAs, enabling modular function design. Build simple functions, then compose them into complex ones.

---

### Example 6: LAMBDA with MAP for Array Processing
```
=MAP(A1:A10, LAMBDA(cell, cell*2+1))
```
**Result:** Array of values where each input is doubled and incremented

**Explanation:** MAP applies a LAMBDA to each element of an array. The LAMBDA receives each cell value and returns the transformed result. Essential for element-wise operations.

---

### Example 7: LAMBDA with REDUCE for Aggregation
```
=REDUCE(0, A1:A10, LAMBDA(accumulator, value, accumulator+value^2))
```
**Result:** Sum of squares of all values in A1:A10

**Explanation:** REDUCE processes an array by accumulating results. The LAMBDA receives the running total (accumulator) and current value, returning the new accumulator. Here it calculates sum of squares.

---

### Example 8: Recursive LAMBDA - Factorial
Name: `FACTORIAL`
```
=LAMBDA(n, IF(n<=1, 1, n*FACTORIAL(n-1)))
```

Usage:
```
=FACTORIAL(5)
```
**Result:** 120

**Explanation:** LAMBDA can call itself for recursion. This calculates n! by multiplying n by factorial of (n-1). Base case returns 1 when n<=1. Must be named to enable self-reference.

---

### Example 9: LAMBDA with BYROW for Row Operations
```
=BYROW(A1:C10, LAMBDA(row, MAX(row)-MIN(row)))
```
**Result:** Array showing range (max-min) for each row

**Explanation:** BYROW passes each row to the LAMBDA as an array. The LAMBDA can perform any row-level calculation. Result is a single-column array with one value per row.

---

### Example 10: LAMBDA with Optional Parameters Using IF
Name: `TAX`
```
=LAMBDA(amount, [rate], amount*IF(ISOMITTED(rate), 0.08, rate))
```

Usage:
```
=TAX(100)        // Returns 8 (uses default 8% rate)
=TAX(100, 0.1)   // Returns 10 (uses provided 10% rate)
```

**Explanation:** Brackets around parameter name make it optional. ISOMITTED() checks if argument was provided. If omitted, use default value. Enables flexible function signatures.

---

### Example 11: LAMBDA with SCAN for Running Calculations
```
=SCAN(0, A1:A10, LAMBDA(prev, current, prev+current))
```
**Result:** Running cumulative sum array

**Explanation:** SCAN is like REDUCE but returns all intermediate values. The LAMBDA receives previous result and current value. Returns array showing cumulative calculation at each step.

---

### Example 12: Complex Business Logic LAMBDA
Name: `SHIPPING_COST`
```
=LAMBDA(weight, zone, express,
  LET(
    base, XLOOKUP(zone, ZoneTable[Zone], ZoneTable[Rate]),
    weight_charge, MAX(weight-1, 0)*0.5,
    subtotal, base + weight_charge,
    IF(express, subtotal*1.5, subtotal)
  )
)
```

Usage:
```
=SHIPPING_COST(5, "West", TRUE)
```

**Explanation:** Combines LAMBDA with LET for readable complex logic. Calculates shipping with zone-based rates, weight surcharge, and express multiplier. All business rules in one named function.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#VALUE!` | Wrong number of arguments passed to LAMBDA | Check parameter count matches arguments provided when calling |
| `#NAME?` | Named LAMBDA not found or parameter name invalid | Verify LAMBDA is saved in Name Manager; parameter names must be valid (no spaces, start with letter) |
| `#CALC!` | Infinite recursion without base case | Add IF condition that returns value without self-call for termination case |
| `LAMBDA function not valid` | Trying to display LAMBDA result directly | LAMBDA must be called with arguments or used with helper functions (MAP, REDUCE, etc.) |
| `Parameter names collision` | Using Excel function names as parameters | Avoid reserved words like SUM, IF, etc. as parameter names |
| `Circular reference` | LAMBDA calls itself without proper naming | Self-referencing LAMBDA must be saved in Name Manager to enable recursion |

## Use Cases

### [[Custom Business Calculations Library]]

**Scenario:** Finance department has complex commission, bonus, and pricing formulas used across dozens of spreadsheets. Each update requires finding and fixing all instances.

**Implementation:**
Create named LAMBDAs:
```
COMMISSION = LAMBDA(sales, tier,
  IFS(tier="Gold", sales*0.15,
      tier="Silver", sales*0.10,
      tier="Bronze", sales*0.07,
      TRUE, sales*0.05))

ANNUAL_BONUS = LAMBDA(base, rating, tenure,
  base * XLOOKUP(rating, RatingTable[Rating], RatingTable[Multiplier])
  * (1 + MIN(tenure, 10)*0.01))
```

**Business Application:** Finance creates a workbook with all named LAMBDAs, distributes it as a template. Every department uses the same `=COMMISSION(B5, C5)` call. When commission structure changes, update one LAMBDA definition and redistribute.

**Technical Details:** Store LAMBDA definitions in a "formulas" workbook that others reference. Consider documenting each LAMBDA with comments in adjacent cells. Test edge cases before deployment.

---

### [[Data Transformation Pipeline]]

**Scenario:** Data imports require consistent cleaning: trim whitespace, convert to proper case, remove special characters, standardize phone formats.

**Implementation:**
```
CLEAN_NAME = LAMBDA(text, PROPER(TRIM(SUBSTITUTE(SUBSTITUTE(text, ".", ""), ",", ""))))

FORMAT_PHONE = LAMBDA(phone,
  LET(digits, TEXTJOIN("", TRUE, IF(ISNUMBER(--MID(phone, SEQUENCE(LEN(phone)), 1)),
                                     MID(phone, SEQUENCE(LEN(phone)), 1), "")),
      "("&LEFT(digits,3)&") "&MID(digits,4,3)&"-"&RIGHT(digits,4)))
```

Usage with arrays:
```
=MAP(RawNames, LAMBDA(x, CLEAN_NAME(x)))
=MAP(RawPhones, LAMBDA(x, FORMAT_PHONE(x)))
```

**Business Application:** Data analysts apply consistent transformations regardless of source format. New team members use documented function names instead of learning complex formulas.

**Technical Details:** Chain LAMBDAs for multi-step transformations. Use MAP to apply across arrays. Consider error handling within LAMBDA for malformed input.

---

### [[Recursive Hierarchy Processing]]

**Scenario:** Organizational chart needs to calculate total headcount under each manager, including all nested levels.

**Implementation:**
```
TOTAL_REPORTS = LAMBDA(manager_id,
  LET(
    direct, SUMIF(OrgTable[ManagerID], manager_id, OrgTable[Headcount]),
    subordinates, FILTER(OrgTable[EmployeeID], OrgTable[ManagerID]=manager_id, ""),
    IF(subordinates="", direct,
       direct + SUM(MAP(subordinates, LAMBDA(sub, TOTAL_REPORTS(sub)))))
  )
)
```

**Business Application:** HR instantly sees full organizational impact of any manager. Useful for restructuring analysis, span of control metrics, and succession planning.

**Technical Details:** Recursion naturally handles unlimited nesting depth. Be cautious of performance with very deep or wide hierarchies. Excel has recursion limits (approximately 1024 depth).

---

### [[Reusable Statistical Functions]]

**Scenario:** Data science team needs weighted averages, moving averages, and custom statistical measures across many analyses.

**Implementation:**
```
WEIGHTED_AVG = LAMBDA(values, weights,
  SUMPRODUCT(values, weights)/SUM(weights))

MOVING_AVG = LAMBDA(data, window,
  MAP(SEQUENCE(ROWS(data)), LAMBDA(i,
    IF(i<window, NA(),
       AVERAGE(INDEX(data, SEQUENCE(window, 1, i-window+1)))))))

COEFFICIENT_VARIATION = LAMBDA(range,
  STDEV(range)/AVERAGE(range)*100)
```

**Business Application:** Standard statistical functions available to all analysts without requiring each to understand underlying formulas. Promotes consistent methodology across analyses.

**Technical Details:** Statistical LAMBDAs should handle edge cases (empty ranges, division by zero). Document expected input formats. Consider returning NA() for insufficient data.

## Platform Differences

### Microsoft Excel
- **Availability:** Excel 365, Excel 2021, Excel for web
- **Storage:** Name Manager (Formulas > Name Manager)
- **Recursion:** Supported when LAMBDA is named
- **Helper functions:** MAP, REDUCE, SCAN, BYROW, BYCOL, MAKEARRAY, ISOMITTED
- **Maximum parameters:** 253

### Google Sheets
- **Availability:** Available since 2022
- **Storage:** Data > Named functions menu
- **Recursion:** Supported through named functions
- **Helper functions:** MAP, REDUCE, SCAN, BYROW, BYCOL, MAKEARRAY, ISOMITTED
- **Maximum parameters:** 253

### Key Differences
The core LAMBDA functionality is identical across platforms. The primary difference is how you save named LAMBDAs:
- **Excel:** Formulas > Name Manager > New > paste LAMBDA formula
- **Sheets:** Data > Named functions > Add new function > define parameters and formula

Inline LAMBDA usage (with MAP, REDUCE, etc.) works identically and formulas are portable.

### Migration Notes
Named LAMBDAs don't automatically transfer between platforms. When moving workbooks, you'll need to recreate the named functions in the new platform's naming system. The LAMBDA formulas themselves can be copied directly.

## Tips and Best Practices

1. **Start simple, then compose:** Build small, single-purpose LAMBDAs first. Combine them into complex functions. `KELVIN` calling `CELSIUS` is easier to debug than one mega-formula.

2. **Use meaningful parameter names:** `LAMBDA(p1, p2, p1*p2)` is harder to understand than `LAMBDA(price, quantity, price*quantity)`. Future you will appreciate self-documenting code.

3. **Always handle edge cases:** What happens when input is blank, zero, or negative? Add IFERROR or IF checks within LAMBDAs for robust production use.

4. **Document your LAMBDAs:** Create a reference sheet in your workbook listing each LAMBDA name, purpose, parameters, and example usage. This is your function library documentation.

5. **Test before deploying:** Validate LAMBDA behavior with various inputs before using in critical calculations. Create a test sheet with expected inputs and outputs.

6. **Use LET inside complex LAMBDAs:** The LET function creates named intermediate values, making complex LAMBDAs readable and preventing redundant calculations.

7. **Be cautious with recursion:** Recursive LAMBDAs are powerful but can hit stack limits or create infinite loops. Always include a base case that returns without self-calling.

8. **Leverage helper functions:** MAP, REDUCE, BYROW, BYCOL, and SCAN are designed to work with LAMBDA. Learn these to unlock LAMBDA's full potential for array operations.

## Related Functions

### Similar Functions
| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[LET]] | Creates named intermediate values | For complex formulas in a single cell without reusability needs |
| [[MAP]] | Applies LAMBDA to each element | When you need element-wise transformations |
| [[REDUCE]] | Aggregates array using LAMBDA | When you need to accumulate a single result from array |
| [[SCAN]] | Like REDUCE but shows all intermediate values | When you need running calculations |
| [[BYROW]] / [[BYCOL]] | Applies LAMBDA to each row/column | When you need row-level or column-level operations |

### Commonly Used Together

**[[MAP]]** - Apply LAMBDA element-by-element

*Transform each cell in a range:*
```
=MAP(Prices, LAMBDA(p, p*1.1))
```
Increases each price by 10%. Essential for element-wise operations.

---

**[[REDUCE]]** - Aggregate with accumulator

*Custom aggregation logic:*
```
=REDUCE(1, Values, LAMBDA(acc, val, acc*val))
```
Multiplies all values together (product function). Second parameter is starting value.

---

**[[LET]]** - Name intermediate calculations

*Inside LAMBDA for clarity:*
```
=LAMBDA(a, b, LET(sum, a+b, diff, a-b, sum*diff))
```
LET prevents recalculating and adds readability within LAMBDA.

---

**[[BYROW]]** - Process each row

*Row-level operations:*
```
=BYROW(Data, LAMBDA(row, TEXTJOIN("-", TRUE, row)))
```
Concatenates each row's values into single strings.

---

**[[MAKEARRAY]]** - Generate array from LAMBDA

*Create computed arrays:*
```
=MAKEARRAY(5, 5, LAMBDA(r, c, r*c))
```
Creates multiplication table. LAMBDA receives row and column numbers.

---

**[[ISOMITTED]]** - Check for optional parameters

*Handle optional arguments:*
```
=LAMBDA(x, [y], IF(ISOMITTED(y), x*2, x*y))
```
Returns different calculation based on whether second argument was provided.

## Official Documentation

- **Microsoft Excel:** [LAMBDA function](https://support.microsoft.com/en-us/office/lambda-function-bd212d27-1cd1-4321-a34a-ccbf254b8b67)
- **Google Sheets:** [LAMBDA function](https://support.google.com/docs/answer/12508718)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel 365 | December 2020 (Insider), March 2021 (General) | Revolutionary addition enabling custom functions |
| Excel 2021 | October 2021 | Included in perpetual license version |
| Excel for web | 2021 | Full feature parity |
| Google Sheets | 2022 | Added as "Named functions" feature |
| Excel 2019 | Not available | Must use alternative approaches (helper columns, VBA) |

---

*Last updated: 2026-01-10*
