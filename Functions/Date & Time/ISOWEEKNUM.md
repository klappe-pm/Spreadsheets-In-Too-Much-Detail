---
categories:
- spreadsheet-functions
subCategories:
- excel
- sheets
topics:
- date-time
subTopics:
- iso-week-number
- week-calculation
- international-standards
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- ISO Week Number
- ISO 8601 Week
- International Week Number
tags:
- isoweeknum
- iso-8601
- week-number
- international
- european-calendar
---

# ISOWEEKNUM

## Description

**ISOWEEKNUM** returns the ISO 8601 week number for a given date. ISO 8601 is the international standard for date and time representation, widely used in Europe, international business, and systems requiring unambiguous date handling. Unlike standard WEEKNUM which always places January 1 in week 1, ISOWEEKNUM follows specific rules: week 1 is the week containing the first Thursday of the year, which is equivalent to the week containing January 4.

The key difference between ISOWEEKNUM and standard WEEKNUM lies in how they handle year boundaries. In ISO week numbering, the first few days of January may belong to the last week of the previous year (week 52 or 53), and the last few days of December may belong to week 1 of the following year. For example, December 31, 2024 (a Tuesday) is in ISO week 1 of 2025, not week 53 of 2024. This can initially seem counterintuitive but ensures that every week has exactly 7 days and weeks don't span year boundaries inconsistently.

**ISO 8601 week definition rules:** A week always starts on Monday and ends on Sunday. Week 1 is the first week with a Thursday in the new year (or equivalently, the week containing January 4, or the first week with at least 4 days in the new year). This means week 1 always starts between December 29 and January 4. Years can have either 52 or 53 weeks—a 53-week year occurs when January 1 is a Thursday, or when January 1 is a Wednesday in a leap year.

ISOWEEKNUM is essential for international business, particularly when working with European partners, filing international reports, or any context where ISO standards are required. Manufacturing, logistics, and supply chain systems commonly use ISO week numbering. The function is often combined with YEAR to create ISO year-week identifiers, though note that the "ISO year" for a date may differ from its calendar year at year boundaries.

## Syntax

> [!f(x)] ISOWEEKNUM Syntax
>
> ```
> =ISOWEEKNUM(date)
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `date` | Yes | The date for which to find the ISO week number. Can be a date value, cell reference containing a date, text in date format, or a function that returns a date. |

### Return Value

Returns an integer from 1 to 53 representing the ISO 8601 week number. Most years have 52 weeks; years where January 1 falls on Thursday (or Wednesday in leap years) have 53 weeks.

## Examples

> [!f(x)] ISOWEEKNUM Examples

### Example 1: Basic ISO Week Number
```
=ISOWEEKNUM(DATE(2026, 1, 10))
```
**Result:** 2

**Explanation:** January 10, 2026 is a Saturday in ISO week 2. Week 1 of 2026 runs from December 29, 2025 to January 4, 2026.

---

### Example 2: First Day of Year
```
=ISOWEEKNUM(DATE(2026, 1, 1))
```
**Result:** 1

**Explanation:** January 1, 2026 is a Thursday, so it falls in ISO week 1 of 2026. Week 1 started December 29, 2025.

---

### Example 3: Year Boundary - December to Week 1
```
=ISOWEEKNUM(DATE(2025, 12, 29))
```
**Result:** 1

**Explanation:** December 29, 2025 (Monday) is the first day of ISO week 1 of 2026. This demonstrates how ISO weeks can span calendar year boundaries.

---

### Example 4: Year Boundary - January to Previous Year
```
=ISOWEEKNUM(DATE(2022, 1, 1))
```
**Result:** 52

**Explanation:** January 1, 2022 is a Saturday. Since this week doesn't contain January 4 (which is Tuesday), it belongs to week 52 of 2021.

---

### Example 5: Compare with Standard WEEKNUM
```
=ISOWEEKNUM(DATE(2022, 1, 1)) & " vs " & WEEKNUM(DATE(2022, 1, 1), 2)
```
**Result:** "52 vs 1"

**Explanation:** Shows the difference between ISO and standard week numbering at year boundaries. WEEKNUM always makes January 1 week 1; ISOWEEKNUM may assign it to week 52/53 of the previous year.

---

### Example 6: Create ISO Year-Week Identifier
```
=YEAR(A1 - WEEKDAY(A1, 2) + 4) & "-W" & TEXT(ISOWEEKNUM(A1), "00")
```
**Result:** "2026-W02" (ISO format year-week)

**Explanation:** Uses the ISO year (which can differ from calendar year at boundaries) combined with ISOWEEKNUM. The formula finds the year of the Thursday in the same week.

---

### Example 7: Determine ISO Year for a Date
```
=YEAR(A1 - WEEKDAY(A1, 2) + 4)
```
**Result:** The ISO year (may differ from YEAR(A1) at year boundaries)

**Explanation:** The ISO year is the year containing the Thursday of the ISO week. This can differ from the calendar year for dates in late December or early January.

---

### Example 8: Check for 53-Week Year
```
=IF(ISOWEEKNUM(DATE(A1, 12, 28)) = 53, "53-week year", "52-week year")
```
**Result:** "53-week year" or "52-week year"

**Explanation:** December 28 is always in the last week of the year (it's the last day guaranteed to be in the current year's weeks). If it's week 53, the year has 53 weeks.

---

### Example 9: Calculate ISO Week Start Date
```
=A1 - WEEKDAY(A1, 2) + 1
```
**Result:** Monday of the ISO week containing the date in A1

**Explanation:** ISO weeks always start on Monday. WEEKDAY with return_type 2 returns Monday=1, so this formula backs up to Monday.

---

### Example 10: Days Remaining in ISO Week
```
=7 - WEEKDAY(A1, 2)
```
**Result:** Number of days until end of ISO week (including current day: 0-6)

**Explanation:** Calculates how many days remain until Sunday (end of ISO week). Monday returns 6, Sunday returns 0.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#VALUE!` | Invalid date input | Ensure input is a valid date value or parseable date text |
| `#VALUE!` | Text that cannot convert to date | Check date format is recognizable (MM/DD/YYYY, YYYY-MM-DD) |
| `#NAME?` | Excel version too old | ISOWEEKNUM requires Excel 2013+; use WEEKNUM(date, 21) as alternative |
| Wrong ISO year | Using YEAR() instead of ISO year | Calculate ISO year: =YEAR(date - WEEKDAY(date, 2) + 4) |
| Confusion at year boundaries | Not understanding ISO rules | January 1 can be in week 52/53 of previous year; December 28-31 can be in week 1 of next year |

## Use Cases

### [[International Business Reporting]]

**Scenario:** Generate weekly reports for European subsidiaries or international partners who require ISO 8601 week numbering for compliance and standardization.

**Implementation:**
```
=YEAR(A1 - WEEKDAY(A1, 2) + 4) & "-W" & TEXT(ISOWEEKNUM(A1), "00")    (ISO identifier)
=SUMIFS(Revenue, ISOWEEKNUM(Dates), $D2, YEAR(Dates - WEEKDAY(Dates, 2) + 4), $E$1)
="CW" & ISOWEEKNUM(TODAY())                                            (Current calendar week)
```

**Business Application:** Multinational corporations consolidate reports using ISO weeks. European factories report production by Kalenderwoche (calendar week). Logistics companies coordinate international shipments using standardized week references.

**Technical Details:** When creating ISO week identifiers, always use the ISO year, not the calendar year. A date like December 31, 2024 would be "2025-W01" not "2024-W01". Calculate ISO year with: =YEAR(date - WEEKDAY(date, 2) + 4).

---

### [[Manufacturing and Production Planning]]

**Scenario:** Schedule production runs, plan material requirements, and track output metrics using ISO week-based planning cycles common in manufacturing environments.

**Implementation:**
```
=ISOWEEKNUM(production_date) - ISOWEEKNUM(planning_start) + 1    (Week of production cycle)
="MRP Week " & ISOWEEKNUM(TODAY()) + lead_time_weeks
=SUMPRODUCT((ISOWEEKNUM(dates) = ISOWEEKNUM(TODAY())) * quantities)
```

**Business Application:** Automotive manufacturers plan production in ISO weeks to coordinate with international suppliers. Pharmaceutical companies schedule batches by ISO week for regulatory tracking. Electronics manufacturers sync component deliveries using global week standards.

**Technical Details:** Many ERP systems (SAP, Oracle) use ISO week numbering internally. Ensure your spreadsheet week references match system conventions to avoid off-by-one-week errors in MRP data.

---

### [[European Union Compliance and Reporting]]

**Scenario:** Meet EU regulatory requirements for periodic reporting that specify ISO 8601 week-based submission schedules and data aggregation.

**Implementation:**
```
=IF(ISOWEEKNUM(TODAY()) = submission_week, "Due This Week", "")
=SUMIFS(Transactions, ISOWEEKNUM(Dates), $C2, YEAR(Dates - WEEKDAY(Dates, 2) + 4), $D$1)
="Report for " & YEAR(A1 - WEEKDAY(A1, 2) + 4) & " Week " & ISOWEEKNUM(A1)
```

**Business Application:** Financial institutions file transaction reports by ISO week. Environmental monitoring uses ISO weeks for emissions data. Labor statistics aggregate hours by ISO calendar week.

**Technical Details:** EU directives often reference "calendar weeks" which means ISO weeks. Ensure your week boundaries align with Monday-Sunday when aggregating data for compliance reports.

## Platform Differences

### Microsoft Excel
- **Availability:** Excel 2013 and later (desktop and 365)
- **Not available:** Excel 2010 and earlier (use WEEKNUM(date, 21) instead)
- **Calculation:** Follows ISO 8601 exactly
- **Week start:** Always Monday (no return_type parameter needed)

### Google Sheets
- **Availability:** All versions
- **Behavior:** Identical to Excel 2013+ implementation
- **Compatibility:** Full support for ISO 8601 standard
- **Interoperability:** Results match Excel's ISOWEEKNUM exactly

### Key Difference Alert
The primary difference is version availability in Excel. For Excel 2010 and earlier, substitute WEEKNUM(date, 21) which produces identical results. Both platforms follow the ISO 8601 standard precisely, so cross-platform compatibility is excellent for ISO week calculations.

## Tips and Best Practices

1. **Always use ISO year with ISOWEEKNUM:** When creating week identifiers, calculate the ISO year: =YEAR(date - WEEKDAY(date, 2) + 4). Using calendar YEAR() leads to incorrect identifiers at year boundaries.

2. **Validate year boundary dates:** Test your formulas with dates like December 31 and January 1 to ensure they handle the ISO week/year correctly when weeks span calendar years.

3. **Use for international consistency:** If working with European partners or international standards, ISOWEEKNUM eliminates ambiguity. "2026-W02" means the same thing worldwide in ISO context.

4. **Fallback for older Excel:** If supporting Excel 2010 or earlier, use: =WEEKNUM(date, 21) which is equivalent to ISOWEEKNUM.

5. **Understand the Thursday rule:** The easiest way to determine which ISO year a date belongs to: find the Thursday of that week and use that year. =YEAR(date - WEEKDAY(date, 2) + 4)

6. **Document your convention:** If your organization uses ISO weeks, document this clearly. Americans often assume Sunday-start weeks; Europeans assume ISO weeks. Explicit labeling prevents confusion.

## Related Functions

### Similar Functions

| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[WEEKNUM]] | Returns week number (various conventions) | When you need Sunday-start weeks or non-ISO conventions |
| [[WEEKDAY]] | Returns day of week (1-7) | When you need the day within the week, not week number |
| [[YEAR]] | Returns calendar year | Use for calendar year; calculate ISO year separately |

### Commonly Used Together

**[[WEEKDAY]]** - Find ISO week start date

*Calculate Monday of any ISO week:*
```
=A1 - WEEKDAY(A1, 2) + 1    // Monday of ISO week
=A1 - WEEKDAY(A1, 2) + 7    // Sunday (end of ISO week)
```
WEEKDAY with return_type 2 uses Monday=1, perfect for ISO week calculations.

---

**[[YEAR]]** - Calculate ISO year

*Determine the ISO year for year-week identifiers:*
```
=YEAR(A1 - WEEKDAY(A1, 2) + 4)    // ISO year (year of Thursday)
=YEAR(A1 - WEEKDAY(A1, 2) + 4) & "-W" & TEXT(ISOWEEKNUM(A1), "00")
```
The ISO year is the calendar year of the Thursday in the same week.

---

**[[TEXT]]** - Format ISO week identifiers

*Create properly formatted ISO week strings:*
```
=TEXT(ISOWEEKNUM(A1), "00")                // "02" with leading zero
="W" & TEXT(ISOWEEKNUM(A1), "00")          // "W02"
```
TEXT ensures single-digit weeks display with leading zeros for proper sorting.

---

**[[DATE]]** - Construct week start dates

*Calculate the first day of a specific ISO week:*
```
=DATE(year, 1, 4) - WEEKDAY(DATE(year, 1, 4), 2) + 1 + (week_num - 1) * 7
```
Finds the Monday of any ISO week number in a given year.

## Official Documentation

- **Microsoft Excel:** [ISOWEEKNUM function](https://support.microsoft.com/en-us/office/isoweeknum-function-1c2d0afe-d25b-4ab1-8894-8d0520e90e0e)
- **Google Sheets:** [ISOWEEKNUM function](https://support.google.com/docs/answer/9061380)
- **ISO 8601:** [ISO 8601 Date and Time Standard](https://www.iso.org/iso-8601-date-and-time-format.html)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel 2013 | 2013 | First introduction as dedicated function |
| Excel 2010 | 2010 | Use WEEKNUM(date, 21) as equivalent |
| Excel 365 | Current | No changes |
| Google Sheets | 2019 | Added for Excel compatibility |
| LibreOffice Calc | 3.5+ | Compatible implementation |

---

*Last updated: 2026-01-10*
