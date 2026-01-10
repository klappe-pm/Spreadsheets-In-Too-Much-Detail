---
categories:
- spreadsheet-functions
subCategories:
- excel
- sheets
topics:
- date-time
subTopics:
- weekday-calculation
- day-of-week
- date-analysis
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Day of Week
- Weekday Number
- Day Index
tags:
- weekday
- day-of-week
- date-analysis
- scheduling
- return-type
---

# WEEKDAY

## Description

**WEEKDAY** returns a number representing the day of the week for a given date. This function is essential for scheduling, conditional formatting, and any analysis that needs to identify or filter by specific days. Whether you need to highlight weekends in a calendar, calculate business days, or schedule recurring events on specific weekdays, WEEKDAY provides the numeric foundation for weekday-based logic.

The function's second parameter, `return_type`, controls which day is considered day 1 and what numbering system is used. This flexibility is crucial because different regions and industries use different week conventions. The United States typically starts weeks on Sunday, while most European countries and ISO 8601 standard start on Monday. Some systems use 0-based indexing (0-6) while others use 1-based (1-7). Understanding these return types is essential for building internationally compatible spreadsheets and integrating with external systems.

**Return type values explained:** Excel and Sheets support multiple return types. Type 1 (default) returns 1 for Sunday through 7 for Saturday. Type 2 returns 1 for Monday through 7 for Sunday. Type 3 returns 0 for Monday through 6 for Sunday (useful for array indexing). Types 11-17 provide additional variations for different regional standards. The most important distinction is whether your week starts on Sunday (American convention) or Monday (European/ISO convention), and whether you want 1-based or 0-based numbering.

WEEKDAY is frequently combined with conditional functions like IF, CHOOSE, and SWITCH to translate numeric results into meaningful text (like "Monday" or "Weekend") or to implement day-specific business logic. It pairs naturally with date arithmetic to find the next or previous occurrence of a specific weekday, and with SUMIFS/COUNTIFS to aggregate data by day of week.

## Syntax

> [!f(x)] WEEKDAY Syntax
>
> ```
> =WEEKDAY(serial_number, [return_type])
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `serial_number` | Yes | The date for which to find the day of the week. Can be a date value, cell reference containing a date, or a function that returns a date. Text strings in date format are also accepted. |
| `return_type` | No | A number that determines the return value system. Controls which day is 1 and whether numbering is 1-based or 0-based. Default is 1 if omitted. See return type table below. |

### Return Type Values

| Return Type | Day 1 Value | Range | Description |
|-------------|-------------|-------|-------------|
| 1 or omitted | Sunday = 1 | 1-7 | Sunday through Saturday (US convention, default) |
| 2 | Monday = 1 | 1-7 | Monday through Sunday (European convention) |
| 3 | Monday = 0 | 0-6 | Monday through Sunday (0-based, array indexing) |
| 11 | Monday = 1 | 1-7 | Same as type 2 |
| 12 | Tuesday = 1 | 1-7 | Tuesday through Monday |
| 13 | Wednesday = 1 | 1-7 | Wednesday through Tuesday |
| 14 | Thursday = 1 | 1-7 | Thursday through Wednesday |
| 15 | Friday = 1 | 1-7 | Friday through Thursday |
| 16 | Saturday = 1 | 1-7 | Saturday through Friday |
| 17 | Sunday = 1 | 1-7 | Sunday through Saturday (same as type 1) |

### Return Value

Returns an integer representing the day of the week. The value depends on the return_type parameter, ranging from 0-6 (type 3) or 1-7 (all other types).

## Examples

> [!f(x)] WEEKDAY Examples

### Example 1: Basic Weekday Detection (Default)
```
=WEEKDAY(DATE(2026, 1, 10))
```
**Result:** 7 (Saturday)

**Explanation:** January 10, 2026 is a Saturday. With default return_type (1), Sunday=1 through Saturday=7, so Saturday returns 7.

---

### Example 2: European Week Convention (Monday Start)
```
=WEEKDAY(DATE(2026, 1, 10), 2)
```
**Result:** 6 (Saturday)

**Explanation:** Using return_type 2, Monday=1 through Sunday=7. Saturday is the 6th day of a Monday-starting week.

---

### Example 3: Zero-Based Indexing for Arrays
```
=WEEKDAY(DATE(2026, 1, 12), 3)
```
**Result:** 0 (Monday)

**Explanation:** Return_type 3 uses 0-based indexing where Monday=0 through Sunday=6. January 12, 2026 is Monday, returning 0. This is ideal for use with CHOOSE or INDEX where the first element is position 0 or 1.

---

### Example 4: Identify Weekend Days
```
=IF(WEEKDAY(A1, 2) > 5, "Weekend", "Weekday")
```
**Result:** "Weekend" if A1 is Saturday or Sunday, else "Weekday"

**Explanation:** Using return_type 2 (Monday=1), Saturday=6 and Sunday=7 are the only values greater than 5. This cleanly separates weekdays (1-5) from weekends (6-7).

---

### Example 5: Get Day Name Using CHOOSE
```
=CHOOSE(WEEKDAY(A1), "Sun", "Mon", "Tue", "Wed", "Thu", "Fri", "Sat")
```
**Result:** Three-letter day abbreviation

**Explanation:** CHOOSE uses the WEEKDAY result (1-7) to select the corresponding day name. With default return_type, position 1 is Sunday, position 7 is Saturday.

---

### Example 6: Get Day Name with Monday Start
```
=CHOOSE(WEEKDAY(A1, 2), "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday", "Sunday")
```
**Result:** Full day name (Monday-start convention)

**Explanation:** Return_type 2 returns 1 for Monday, so CHOOSE position 1 should be "Monday". This matches European conventions.

---

### Example 7: Find Next Monday
```
=A1 + 8 - WEEKDAY(A1, 2)
```
**Result:** Date of the next Monday after the date in A1

**Explanation:** WEEKDAY(A1, 2) returns 1-7 (Mon-Sun). Subtracting this from 8 gives the days until next Monday. If A1 is already Monday, this returns 7 days later; adjust with MOD for same-day handling.

---

### Example 8: Find Previous Monday (Week Start)
```
=A1 - WEEKDAY(A1, 2) + 1
```
**Result:** The Monday of the week containing the date in A1

**Explanation:** This calculates the Monday at the start of the current week. If A1 is Monday (weekday=1), result is A1. If A1 is Wednesday (weekday=3), goes back 2 days to Monday.

---

### Example 9: Calculate Week Start (Sunday)
```
=A1 - WEEKDAY(A1) + 1
```
**Result:** The Sunday at the start of the week containing A1

**Explanation:** With default return_type, Sunday=1. Subtracting WEEKDAY and adding 1 always returns the Sunday of that week.

---

### Example 10: Conditional Formatting Formula for Weekends
```
=WEEKDAY(A1, 2) > 5
```
**Result:** TRUE for Saturday/Sunday, FALSE otherwise

**Explanation:** Apply this as a conditional formatting formula to highlight weekend dates. Using return_type 2, only Saturday (6) and Sunday (7) exceed 5.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#VALUE!` | Non-date value in serial_number | Ensure input is a valid date value or text that can be parsed as a date |
| `#NUM!` | Invalid return_type value | Use only valid return_type values: 1, 2, 3, or 11-17 |
| `#VALUE!` | Text that cannot be converted to date | Verify date text is in a recognizable format (MM/DD/YYYY, YYYY-MM-DD, etc.) |
| Unexpected result | Wrong return_type for your convention | Verify you're using the correct return_type for your week start day |
| Off-by-one errors | Confusing 0-based vs 1-based indexing | Type 3 is 0-based (0-6); all others are 1-based (1-7) |

## Use Cases

### [[Staff Scheduling and Shift Management]]

**Scenario:** Create a work schedule that automatically identifies which employees work each day based on their assigned weekday patterns (e.g., Mon-Wed-Fri or Tue-Thu-Sat rotations).

**Implementation:**
```
=IF(MOD(WEEKDAY(B$2, 2), 2) = $A3, "WORK", "")     (Alternating days)
=IF(OR(WEEKDAY(B$2, 2) = 1, WEEKDAY(B$2, 2) = 3, WEEKDAY(B$2, 2) = 5), "MWF", "")
=SUMPRODUCT((WEEKDAY(date_range, 2) <= 5) * hours)    (Total weekday hours)
```

**Business Application:** Restaurant managers assign servers to specific weekdays. Retail stores staff differently on weekends versus weekdays. Healthcare facilities ensure coverage patterns match expected patient volumes by day of week.

**Technical Details:** Using return_type 2 makes weekday logic cleaner since Monday-Friday are 1-5 and Saturday-Sunday are 6-7. This allows simple comparisons like WEEKDAY(date, 2) <= 5 for weekdays.

---

### [[Business Day Calculations]]

**Scenario:** Calculate deadlines that exclude weekends, count business days in a range, or identify the next business day after a given date.

**Implementation:**
```
=IF(WEEKDAY(A1, 2) > 5, A1 + (8 - WEEKDAY(A1, 2)), A1)    (Next business day)
=SUMPRODUCT((WEEKDAY(date_range, 2) <= 5) * 1)           (Count weekdays)
=A1 + QUOTIENT(days, 5) * 7 + MOD(days, 5)               (Add business days approx)
```

**Business Application:** Legal filings have deadlines measured in business days. Financial settlements require T+2 business day calculations. SLA metrics track business day response times.

**Technical Details:** For complex business day calculations with holidays, NETWORKDAYS and WORKDAY functions are more appropriate. WEEKDAY is useful for simple weekend detection or when building custom logic not supported by those functions.

---

### [[Data Analysis by Day of Week]]

**Scenario:** Analyze sales, traffic, or other metrics by day of week to identify patterns, peak days, and opportunities.

**Implementation:**
```
=SUMIFS(sales, WEEKDAY(dates, 2), 1)                     (Monday sales total)
=AVERAGEIFS(visitors, WEEKDAY(dates, 2), 6)              (Saturday average)
=COUNTIFS(orders, ">0", WEEKDAY(dates, 2), {6,7})        (Weekend order count)
```

**Business Application:** E-commerce sites identify their busiest shopping days. Content publishers determine optimal posting schedules. Customer service teams staff based on day-of-week call volumes.

**Technical Details:** Create helper columns with WEEKDAY values to enable pivot table analysis by day of week. Use CHOOSE to convert numbers to day names for cleaner reports.

## Platform Differences

### Microsoft Excel
- **Availability:** All versions since Excel 97
- **Return types:** Supports all return types (1, 2, 3, 11-17)
- **Date handling:** Accepts serial numbers, date values, and text in date format
- **Time component:** Ignores time portion of datetime values

### Google Sheets
- **Availability:** All versions
- **Return types:** Supports all the same return types as Excel
- **Date handling:** Same flexibility as Excel for input formats
- **Behavior:** Functionally identical to Excel

### Key Difference Alert
Both platforms handle WEEKDAY identically. The main consideration is ensuring consistent return_type usage when sharing workbooks between regions with different week-start conventions. European users naturally expect type 2 (Monday start); American users expect type 1 (Sunday start).

## Tips and Best Practices

1. **Choose return_type 2 for business logic:** Monday=1 through Friday=5 makes weekday/weekend checks much cleaner (<=5 for weekdays, >5 for weekends). This is more intuitive than default type 1 where Sunday and Saturday are 1 and 7.

2. **Document your return_type choice:** When building templates used by international teams, add a comment or note indicating which week convention you're using. What's obvious to you may confuse colleagues in other regions.

3. **Use CHOOSE for readable day names:** Rather than nested IF statements, =CHOOSE(WEEKDAY(date, 2), "Mon", "Tue", "Wed", "Thu", "Fri", "Sat", "Sun") is cleaner and easier to maintain.

4. **Combine with TEXT for locale-aware names:** =TEXT(date, "dddd") returns the full day name using system locale settings, avoiding hardcoded day names that break in other languages.

5. **Use type 3 for 0-based array work:** When WEEKDAY feeds into array formulas or INDEX functions expecting 0-based positions, type 3 (Monday=0 through Sunday=6) may simplify your formulas.

6. **Build week-start calculations systematically:** =date - WEEKDAY(date, return_type) + 1 always gives the first day of the week for that return_type. Memorize this pattern.

## Related Functions

### Similar Functions

| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[WEEKNUM]] | Returns week number of the year | When you need the week number (1-52) rather than day of week (1-7) |
| [[ISOWEEKNUM]] | Returns ISO 8601 week number | When you need standardized international week numbers |
| [[TEXT]] | Converts date to formatted text | When you need day names ("Monday") with locale awareness |

### Commonly Used Together

**[[CHOOSE]]** - Convert numbers to text

*Get day names from WEEKDAY numbers:*
```
=CHOOSE(WEEKDAY(A1), "Sun", "Mon", "Tue", "Wed", "Thu", "Fri", "Sat")
=CHOOSE(WEEKDAY(A1, 2), "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday", "Sunday")
```
CHOOSE is the cleanest way to convert WEEKDAY's numeric output to readable day names.

---

**[[IF]]** - Conditional weekday logic

*Apply different rules for different days:*
```
=IF(WEEKDAY(A1, 2) <= 5, "Business Day", "Weekend")
=IF(WEEKDAY(A1, 2) = 1, "Monday - Weekly Meeting", "")
```
IF transforms WEEKDAY output into actionable decisions.

---

**[[SUMIFS]] / [[COUNTIFS]]** - Aggregate by weekday

*Analyze data by day of week:*
```
=SUMIFS(B:B, WEEKDAY(A:A, 2), 1)    // Sum all Monday values
=COUNTIFS(WEEKDAY(A:A, 2), ">5")    // Count weekend entries
```
Filter and aggregate data based on which day of the week dates fall on.

---

**[[NETWORKDAYS]]** - Business day counting

*Combine with WEEKDAY for custom logic:*
```
=NETWORKDAYS(start, end) - holiday_count
=IF(WEEKDAY(end, 2) > 5, NETWORKDAYS(start, end + 2 - WEEKDAY(end, 2) + 5), NETWORKDAYS(start, end))
```
When NETWORKDAYS doesn't match your business rules, supplement with WEEKDAY logic.

## Official Documentation

- **Microsoft Excel:** [WEEKDAY function](https://support.microsoft.com/en-us/office/weekday-function-60e44483-2ed1-439f-8bd0-e404c190949a)
- **Google Sheets:** [WEEKDAY function](https://support.google.com/docs/answer/3093296)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel 97 | 1997 | Original with return_types 1, 2, 3 |
| Excel 2010 | 2010 | Added return_types 11-17 for extended week start options |
| Excel 365 | Current | No changes |
| Google Sheets | Original launch (2006) | All return types supported from launch |
| LibreOffice Calc | All versions | Compatible implementation |

---

*Last updated: 2026-01-10*
