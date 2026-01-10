---
categories:
- spreadsheet-functions
subCategories:
- excel
- sheets
topics:
- date-time
subTopics:
- week-number
- week-calculation
- calendar-analysis
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Week Number
- Week of Year
- Calendar Week
tags:
- weeknum
- week-number
- calendar-week
- date-analysis
- return-type
---

# WEEKNUM

## Description

**WEEKNUM** returns the week number of a specific date within the year. This function is essential for weekly reporting, scheduling, inventory management, and any analysis that groups data by week. Whether you need to summarize sales by week, track project milestones on a weekly basis, or create fiscal calendars, WEEKNUM provides the numeric week identifier (typically 1-53) that enables week-based data organization.

The function's second parameter, `return_type`, controls which day of the week is considered the start of a new week. This flexibility matters because different countries and industries use different week-start conventions. The United States typically considers Sunday as the first day of the week, while most European countries and the ISO 8601 standard use Monday. The return_type parameter allows you to align WEEKNUM with your organization's or region's convention.

**Understanding week numbering:** Week 1 of a year is the week containing January 1 (in most return_type modes). This means January 1 is always in week 1, regardless of which day of the week it falls on. However, this can lead to confusion at year boundaries—December 31 might be in week 53 of the current year, while January 1 of the next day is in week 1 of the new year. For ISO 8601-compliant week numbering where week 1 is defined differently (the week containing the first Thursday), use ISOWEEKNUM instead.

WEEKNUM pairs naturally with YEAR to create unique week identifiers (like "2026-W03") and with SUMIFS/COUNTIFS to aggregate data by week. It's commonly used in pivot tables, charts, and reports where weekly granularity provides the right balance between daily detail and monthly summary. When combined with date arithmetic, you can calculate week start dates, week end dates, and the number of weeks between dates.

## Syntax

> [!f(x)] WEEKNUM Syntax
>
> ```
> =WEEKNUM(serial_number, [return_type])
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `serial_number` | Yes | The date for which to find the week number. Can be a date value, cell reference, text in date format, or a function returning a date. |
| `return_type` | No | A number indicating which day the week begins. Default is 1 (week starts on Sunday). See return type table below. |

### Return Type Values

| Return Type | Week Starts On | Description |
|-------------|----------------|-------------|
| 1 or omitted | Sunday | US convention (default) |
| 2 | Monday | European convention |
| 11 | Monday | Same as 2 |
| 12 | Tuesday | Week begins Tuesday |
| 13 | Wednesday | Week begins Wednesday |
| 14 | Thursday | Week begins Thursday |
| 15 | Friday | Week begins Friday |
| 16 | Saturday | Week begins Saturday |
| 17 | Sunday | Same as 1 |
| 21 | Monday | ISO 8601 compliant (same as ISOWEEKNUM) |

### Return Value

Returns an integer from 1 to 54 indicating the week number. Most years have 52 or 53 weeks. The exact week number depends on which day the year starts and the return_type chosen.

## Examples

> [!f(x)] WEEKNUM Examples

### Example 1: Basic Week Number (Default Sunday Start)
```
=WEEKNUM(DATE(2026, 1, 10))
```
**Result:** 2

**Explanation:** January 10, 2026 is a Saturday. With weeks starting on Sunday (default), this date falls in the second week of 2026 (January 4-10).

---

### Example 2: European Convention (Monday Start)
```
=WEEKNUM(DATE(2026, 1, 10), 2)
```
**Result:** 2

**Explanation:** Using return_type 2 (Monday start), January 10, 2026 falls in week 2 (January 5-11). The specific week number may differ from Sunday-start depending on the year.

---

### Example 3: First Week of Year
```
=WEEKNUM(DATE(2026, 1, 1))
```
**Result:** 1

**Explanation:** January 1 is always in week 1 with standard WEEKNUM behavior. This differs from ISO week numbering where week 1 has specific rules.

---

### Example 4: Last Week of Year
```
=WEEKNUM(DATE(2026, 12, 31))
```
**Result:** 53

**Explanation:** December 31, 2026 falls in week 53. Some years have 53 weeks when the year starts or ends mid-week.

---

### Example 5: Create Year-Week Identifier
```
=YEAR(A1) & "-W" & TEXT(WEEKNUM(A1, 2), "00")
```
**Result:** "2026-W02" (for January 10, 2026)

**Explanation:** Combines YEAR and WEEKNUM to create a sortable week identifier. The TEXT function ensures single-digit weeks have a leading zero (W01 not W1).

---

### Example 6: ISO-Compatible Week Number
```
=WEEKNUM(DATE(2026, 1, 1), 21)
```
**Result:** 1 (or 52/53 in years where Jan 1 is late in the week)

**Explanation:** Return_type 21 uses ISO 8601 rules where week 1 is the week containing the first Thursday. Equivalent to ISOWEEKNUM function.

---

### Example 7: Count Weeks Between Dates
```
=WEEKNUM(B1) - WEEKNUM(A1)
```
**Result:** Number of week boundaries between two dates

**Explanation:** Subtracting week numbers gives the number of weeks spanned. Note: This doesn't count complete 7-day periods; it counts week boundary crossings.

---

### Example 8: Week Start Date Calculation
```
=A1 - WEEKDAY(A1, 2) + 1
```
**Result:** Monday of the week containing the date in A1

**Explanation:** Combined with WEEKNUM, this helps identify week boundaries. This formula finds the Monday of any given week.

---

### Example 9: Compare Same Week Year-Over-Year
```
=SUMIFS(Sales, WEEKNUM(Dates, 2), WEEKNUM(TODAY(), 2), YEAR(Dates), YEAR(TODAY())-1)
```
**Result:** Sum of sales from the same week number last year

**Explanation:** Uses WEEKNUM to compare performance in the same calendar week across years. Useful for seasonal analysis.

---

### Example 10: Group by Quarter and Week
```
=CHOOSE(CEILING(WEEKNUM(A1)/13, 1), "Q1", "Q2", "Q3", "Q4") & " Week " & MOD(WEEKNUM(A1)-1, 13)+1
```
**Result:** "Q1 Week 2" (position within quarter)

**Explanation:** Divides the year into quarters of approximately 13 weeks each, showing both the quarter and the week within that quarter.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#VALUE!` | Non-date input in serial_number | Ensure input is a valid date value or recognized date text |
| `#NUM!` | Invalid return_type | Use valid return_type values: 1, 2, 11-17, or 21 |
| `#VALUE!` | Empty cell reference | Handle empty cells with IF(ISBLANK(...)) before WEEKNUM |
| Unexpected week number | Year boundary confusion | Remember Jan 1 is always week 1 in standard WEEKNUM (not ISO) |
| Different results than expected | Wrong return_type for your convention | Verify you're using the correct return_type for your week start |

## Use Cases

### [[Weekly Sales and Performance Reporting]]

**Scenario:** Create weekly summary reports that aggregate daily transaction data into weekly totals for trend analysis and management dashboards.

**Implementation:**
```
=SUMIFS(B:B, WEEKNUM(A:A, 2), $D2, YEAR(A:A), $E$1)    (Sum by week)
=AVERAGEIFS(B:B, WEEKNUM(A:A, 2), $D2)                  (Weekly average)
=COUNTIFS(A:A, ">0", WEEKNUM(A:A, 2), WEEKNUM(TODAY(), 2))  (This week count)
```

**Business Application:** Retail managers track weekly sales trends to identify patterns. Marketing teams measure campaign performance by week. Operations teams monitor weekly productivity metrics.

**Technical Details:** Create a helper column with WEEKNUM values to enable pivot table grouping by week. Combine with YEAR to handle year boundaries correctly: =YEAR(A1) & "-" & TEXT(WEEKNUM(A1, 2), "00").

---

### [[Inventory and Supply Chain Management]]

**Scenario:** Plan inventory levels, schedule deliveries, and track lead times on a weekly basis for warehouse and supply chain operations.

**Implementation:**
```
=WEEKNUM(delivery_date, 2) - WEEKNUM(order_date, 2)     (Lead time in weeks)
=SUMIFS(orders, WEEKNUM(dates, 2), WEEKNUM(TODAY(), 2)+1)  (Next week's orders)
=IF(WEEKNUM(A1, 2) = WEEKNUM(TODAY(), 2), "This Week", "Future")
```

**Business Application:** Warehouses plan receiving schedules by week. Procurement teams forecast weekly demand. Logistics coordinators schedule weekly shipping batches.

**Technical Details:** For lead times spanning year boundaries, use: =INT((B1-A1)/7) for actual weeks elapsed rather than WEEKNUM subtraction.

---

### [[Project and Sprint Planning]]

**Scenario:** Organize project timelines into weekly sprints, track milestone dates by week, and report progress on a weekly cadence.

**Implementation:**
```
="Sprint " & WEEKNUM(A1, 2) - WEEKNUM(project_start, 2) + 1
=IF(WEEKNUM(due_date, 2) <= WEEKNUM(TODAY(), 2), "Past Due", "On Track")
=WEEKNUM(end_date, 2) - WEEKNUM(start_date, 2) + 1       (Duration in weeks)
```

**Business Application:** Agile teams number sprints using week numbers. Project managers report weekly status against baselines. Resource planning aligns with weekly capacity.

**Technical Details:** Consider using ISOWEEKNUM for international teams where ISO week numbering is standard. Create consistent week identifiers that work across year boundaries.

## Platform Differences

### Microsoft Excel
- **Availability:** All versions since Excel 2000; extended return_types (11-17, 21) added in Excel 2010
- **Return types:** Full support for all return_types including 21 (ISO)
- **Week 1 definition:** Week containing January 1 (except return_type 21)
- **Maximum weeks:** Can return up to 54 in rare year configurations

### Google Sheets
- **Availability:** All versions
- **Return types:** Supports all the same return_types as Excel
- **Behavior:** Functionally identical to Excel
- **Locale handling:** Same week start conventions apply

### Key Difference Alert
Both platforms calculate WEEKNUM identically. The critical consideration is choosing between WEEKNUM and ISOWEEKNUM based on your organization's standards. Many international companies require ISO week numbering (use ISOWEEKNUM or WEEKNUM with return_type 21). US-based organizations typically use standard WEEKNUM with Sunday or Monday start.

## Tips and Best Practices

1. **Use return_type 2 for business contexts:** Most business calendars run Monday-Friday, so starting weeks on Monday (return_type 2) aligns WEEKNUM with business week thinking.

2. **Create year-week identifiers:** =YEAR(date) & "-W" & TEXT(WEEKNUM(date, 2), "00") creates sortable identifiers like "2026-W03" that handle year boundaries correctly.

3. **Handle year boundaries carefully:** WEEKNUM(Dec 31) might be 53 while WEEKNUM(Jan 1) is 1. For continuous week counting, consider using date arithmetic: =INT((date - start_date)/7)+1.

4. **Know when to use ISOWEEKNUM:** If your organization follows ISO 8601 or operates internationally with European partners, ISOWEEKNUM provides standardized week numbering that matches international conventions.

5. **Add helper columns for pivot tables:** Pivot tables group more easily when you have explicit WEEKNUM and YEAR columns rather than trying to group by date fields.

6. **Combine with WEEKDAY for precision:** To find the exact date of the Monday of week 5: =DATE(year, 1, 1) + (5-1)*7 - WEEKDAY(DATE(year, 1, 1), 2) + 1.

## Related Functions

### Similar Functions

| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[ISOWEEKNUM]] | Returns ISO 8601 week number | When international standardization is required |
| [[WEEKDAY]] | Returns day of week (1-7) | When you need the day within the week, not the week number |
| [[YEAR]] | Returns year from date | Often combined with WEEKNUM for unique week identifiers |

### Commonly Used Together

**[[YEAR]]** - Create unique week identifiers

*Build sortable year-week codes:*
```
=YEAR(A1) & "-W" & TEXT(WEEKNUM(A1, 2), "00")
=YEAR(A1) * 100 + WEEKNUM(A1, 2)    // Numeric identifier like 202602
```
Combining YEAR and WEEKNUM prevents week number ambiguity across years.

---

**[[SUMIFS]] / [[COUNTIFS]]** - Aggregate by week

*Summarize data by week number:*
```
=SUMIFS(Sales, WEEKNUM(Dates, 2), D2, YEAR(Dates), E2)
=COUNTIFS(WEEKNUM(A:A, 2), WEEKNUM(TODAY(), 2))
```
Filter and aggregate data based on which week dates fall in.

---

**[[TEXT]]** - Format week numbers

*Display formatted week identifiers:*
```
=TEXT(WEEKNUM(A1), "00")                    // "02" instead of "2"
="Week " & TEXT(WEEKNUM(A1, 2), "00")       // "Week 02"
```
TEXT ensures consistent formatting with leading zeros for sorting and display.

---

**[[ISOWEEKNUM]]** - ISO-compliant alternative

*Compare standard vs ISO week numbering:*
```
=WEEKNUM(A1, 21)    // Equivalent to ISOWEEKNUM(A1)
=IF(ISOWEEKNUM(A1) <> WEEKNUM(A1, 2), "ISO differs", "Same")
```
Use when you need to reconcile different week numbering systems.

## Official Documentation

- **Microsoft Excel:** [WEEKNUM function](https://support.microsoft.com/en-us/office/weeknum-function-e5c43a03-b4ab-426c-b411-b18c13c75340)
- **Google Sheets:** [WEEKNUM function](https://support.google.com/docs/answer/3294949)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel 2000 | 2000 | Original with return_types 1, 2 |
| Excel 2010 | 2010 | Added return_types 11-17 and 21 (ISO) |
| Excel 365 | Current | No changes to function behavior |
| Google Sheets | Original launch (2006) | All return types supported |
| LibreOffice Calc | All versions | Compatible implementation |

---

*Last updated: 2026-01-10*
