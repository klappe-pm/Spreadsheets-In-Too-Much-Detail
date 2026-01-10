---
categories:
- spreadsheet-functions
subCategories:
- excel
- sheets
topics:
- date-time
subTopics:
- day-counting
- date-difference
- date-arithmetic
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Days Between
- Day Count
- Date Difference
tags:
- days
- day-count
- date-difference
- date-arithmetic
- duration
---

# DAYS

## Description

**DAYS** returns the number of days between two dates. This function provides the simplest and most direct way to calculate the elapsed time between two points in time, measured in whole calendar days. Unlike NETWORKDAYS which counts only business days, DAYS counts every single day including weekends and holidays.

The function subtracts the start date from the end date and returns the result. The calculation is inclusive of neither endpoint by default—it gives the number of days between the dates, not including both dates themselves. DAYS(January 1, January 10) returns 9, representing the 9 days that pass to get from January 1 to January 10. This is equivalent to simple date subtraction (end_date - start_date) but provides a cleaner, more readable formula.

**Why use DAYS over subtraction?** While =B1-A1 gives the same result as =DAYS(B1, A1), the DAYS function is clearer in intent, works directly with date text strings, and provides consistent behavior when dates are stored in different formats. DAYS also handles the argument order logically—DAYS(end, start) returns a positive number when end is after start, and negative when end is before start.

DAYS is commonly used for age calculations, project duration tracking, counting down to events, and any scenario requiring the total elapsed calendar time. It pairs with NETWORKDAYS for business day comparisons, with YEARFRAC for expressing durations as fractional years, and with DATEDIF for more complex duration components (years, months, days separately).

## Syntax

> [!f(x)] DAYS Syntax
>
> ```
> =DAYS(end_date, start_date)
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `end_date` | Yes | The later date (ending point of the period). |
| `start_date` | Yes | The earlier date (starting point of the period). |

### Return Value

Returns an integer representing the number of days between the two dates. Returns a positive number when end_date is after start_date, and a negative number when end_date is before start_date.

## Examples

> [!f(x)] DAYS Examples

### Example 1: Basic Days Between
```
=DAYS(DATE(2026, 1, 15), DATE(2026, 1, 1))
```
**Result:** 14

**Explanation:** From January 1 to January 15 is 14 days. Note: January 1 is the start, so 14 days pass to reach January 15.

---

### Example 2: Days Until Event
```
=DAYS(DATE(2026, 12, 25), TODAY())
```
**Result:** Days remaining until Christmas 2026

**Explanation:** Counts down to a future event. Result decreases by 1 each day as TODAY() advances.

---

### Example 3: Days Since Event
```
=DAYS(TODAY(), DATE(2020, 1, 1))
```
**Result:** Days since January 1, 2020

**Explanation:** Counts up from a past event. Result increases by 1 each day.

---

### Example 4: Negative Result (Reversed Dates)
```
=DAYS(DATE(2026, 1, 1), DATE(2026, 1, 15))
```
**Result:** -14

**Explanation:** When end_date is before start_date, the result is negative.

---

### Example 5: Same Day
```
=DAYS(DATE(2026, 1, 10), DATE(2026, 1, 10))
```
**Result:** 0

**Explanation:** The same date has zero days between—no time passes.

---

### Example 6: One Year
```
=DAYS(DATE(2027, 1, 1), DATE(2026, 1, 1))
```
**Result:** 365

**Explanation:** 2026 is not a leap year, so one year equals 365 days.

---

### Example 7: Leap Year Consideration
```
=DAYS(DATE(2025, 1, 1), DATE(2024, 1, 1))
```
**Result:** 366

**Explanation:** 2024 is a leap year, so the year has 366 days.

---

### Example 8: Using Text Dates
```
=DAYS("2026-06-15", "2026-01-01")
```
**Result:** 165

**Explanation:** DAYS can parse recognizable date text strings directly.

---

### Example 9: Age in Days
```
=DAYS(TODAY(), birth_date)
```
**Result:** Person's age in total days

**Explanation:** Simple age calculation. Divide by 365.25 for approximate years.

---

### Example 10: Project Duration
```
=DAYS(project_end, project_start)
```
**Result:** Total calendar days of project

**Explanation:** Measures complete project duration regardless of weekends or holidays.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#VALUE!` | Invalid date in either parameter | Ensure both values are valid dates |
| `#VALUE!` | Text that cannot be parsed as date | Use DATE function or recognized date format |
| Negative when positive expected | Arguments in wrong order | DAYS expects (end_date, start_date), not (start_date, end_date) |
| Off by one | Expecting inclusive counting | DAYS gives days between; add 1 if you want to include both endpoints |
| Decimal result | Input dates have time components | DAYS uses only the date portion; times are ignored |

## Use Cases

### [[Age and Tenure Calculations]]

**Scenario:** Calculate ages, service tenure, and elapsed time from significant dates for HR, benefits, and personal applications.

**Implementation:**
```
=DAYS(TODAY(), birth_date)                           (Age in days)
=INT(DAYS(TODAY(), hire_date) / 365.25)             (Approximate years of service)
=DAYS(TODAY(), start_date) & " days since we met"
```

**Business Application:** HR systems calculate employee tenure for benefits eligibility. Customer relationship management tracks account age. Personal planners count days to milestones.

**Technical Details:** For precise age in years, DATEDIF is more accurate than dividing by 365.25. However, for total elapsed days, DAYS is the simplest and most accurate function.

---

### [[Project Tracking and Duration]]

**Scenario:** Measure project durations, track elapsed time, and calculate remaining time for planning and reporting.

**Implementation:**
```
=DAYS(actual_end, actual_start)                      (Actual duration)
=DAYS(planned_end, planned_start) - DAYS(actual_end, actual_start)  (Variance)
=DAYS(deadline, TODAY())                             (Days remaining)
```

**Business Application:** Project managers report on duration metrics. Resource planners calculate timeline impacts. Status dashboards show countdown to milestones.

**Technical Details:** DAYS counts calendar days, not working days. For business day calculations, combine with NETWORKDAYS or use DAYS for calendar duration and NETWORKDAYS for work duration.

---

### [[Event Countdown and Anniversary Tracking]]

**Scenario:** Count days until future events, track time since past events, and manage anniversary and milestone recognition.

**Implementation:**
```
=DAYS(event_date, TODAY())                           (Days until event)
=IF(DAYS(event_date, TODAY()) < 0, "Passed", DAYS(event_date, TODAY()) & " days to go")
=DAYS(TODAY(), anniversary) & " days since anniversary"
```

**Business Application:** Marketing teams count down to product launches. Event planners track preparation timelines. Recognition programs identify upcoming anniversaries.

**Technical Details:** Handle past events gracefully—DAYS returns negative when the event has passed. Use ABS() if you need the magnitude regardless of direction.

## Platform Differences

### Microsoft Excel
- **Availability:** Excel 2013 and later
- **Not available:** Excel 2010 and earlier (use simple subtraction instead)
- **Date handling:** Accepts serial numbers, date values, and text
- **Time components:** Ignores time; uses only date portion

### Google Sheets
- **Availability:** All versions
- **Behavior:** Identical to Excel 2013+
- **Fallback:** For older Excel compatibility, use =end_date - start_date

### Key Difference Alert
DAYS was introduced in Excel 2013. For compatibility with Excel 2010 and earlier, use simple date subtraction: =B1-A1 produces the same result as =DAYS(B1, A1). The main advantage of DAYS is readability and explicit intent.

## Tips and Best Practices

1. **Remember the parameter order:** DAYS takes (end_date, start_date), not (start_date, end_date). This is the opposite of what you might expect and differs from DATEDIF.

2. **Use for simple counts only:** DAYS counts all calendar days. If you need business days, use NETWORKDAYS. If you need years/months/days, use DATEDIF.

3. **Handle negative results:** When dates might be in either order, use ABS(DAYS(A1, B1)) to get the magnitude regardless of which date is later.

4. **Fall back to subtraction if needed:** For Excel 2010 compatibility, =end_date - start_date is equivalent to DAYS.

5. **Don't confuse with inclusive counting:** DAYS gives days between, not including both endpoints. If you need inclusive counting (how many days in the range), add 1 to the result.

6. **Combine with INT for approximate years:** =INT(DAYS(end, start)/365.25) gives approximate years. Use DATEDIF for precise year/month counting.

## Related Functions

### Similar Functions

| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[NETWORKDAYS]] | Counts business days only | When you need to exclude weekends and holidays |
| [[DATEDIF]] | Returns years, months, or days | When you need component-based differences |
| [[DAYS360]] | Counts using 30-day months | For financial calculations using 30/360 |

### Commonly Used Together

**[[TODAY]]** - Dynamic date reference

*Create live countdowns and counts:*
```
=DAYS(event_date, TODAY())          // Days until event
=DAYS(TODAY(), start_date)          // Days since start
```
TODAY() makes DAYS calculations update automatically.

---

**[[NETWORKDAYS]]** - Business day comparison

*Compare total vs business days:*
```
=DAYS(B1, A1)                       // Total calendar days
=NETWORKDAYS(A1, B1)                // Business days only
=DAYS(B1, A1) - NETWORKDAYS(A1, B1) // Weekend days in range
```
Understand how much of total time is working time.

---

**[[DATEDIF]]** - Component-based duration

*Get years, months, days separately:*
```
=DATEDIF(A1, B1, "Y")               // Complete years
=DATEDIF(A1, B1, "M")               // Complete months
=DAYS(B1, A1)                       // Total days
```
DAYS gives total days; DATEDIF breaks it into components.

---

**[[INT]]** - Convert to approximate years

*Calculate approximate years from days:*
```
=INT(DAYS(B1, A1) / 365.25)         // Approximate years
=DAYS(B1, A1) / 7                   // Weeks
=DAYS(B1, A1) / 30.44               // Approximate months
```
Divide day count by period length for unit conversion.

## Official Documentation

- **Microsoft Excel:** [DAYS function](https://support.microsoft.com/en-us/office/days-function-57740535-d549-4395-8728-0f07bff0b9df)
- **Google Sheets:** [DAYS function](https://support.google.com/docs/answer/3093041)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel 2013 | 2013 | First introduction |
| Excel 2016+ | 2016 | No changes |
| Excel 365 | Current | No changes |
| Google Sheets | Original launch (2006) | Available since launch |
| LibreOffice Calc | All versions | Compatible implementation |

---

*Last updated: 2026-01-10*
