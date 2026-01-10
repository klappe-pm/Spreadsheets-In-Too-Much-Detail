---
categories:
- spreadsheet-functions
subCategories:
- excel
- sheets
topics:
- date-time
subTopics:
- business-days
- workday-calculation
- date-arithmetic
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Business Day
- Working Day
- Add Business Days
tags:
- workday
- business-days
- date-calculation
- scheduling
- holidays
---

# WORKDAY

## Description

**WORKDAY** returns a date that is a specified number of working days before or after a starting date. Working days are defined as all days except Saturdays, Sundays, and any dates you specify as holidays. This function is essential for calculating deadlines, delivery dates, project milestones, and any scheduling that needs to account for business days rather than calendar days.

The function automatically skips weekends (Saturday and Sunday) when counting days, making it ideal for standard Monday-Friday business operations. When you specify 10 working days from a start date, WORKDAY counts forward, skipping every Saturday and Sunday it encounters. If it lands on a weekend, it continues to the next working day. This behavior eliminates the complex manual calculations required to determine "what date is 15 business days from today?"

**Holiday handling is critical:** The optional holidays parameter accepts a range of dates to exclude in addition to weekends. If your organization observes Christmas, New Year's, and other holidays, include them in a range and WORKDAY will skip those dates just like weekends. Building a comprehensive holiday list is essential for accurate business day calculations—without it, WORKDAY only excludes weekends.

WORKDAY pairs naturally with NETWORKDAYS (which counts business days between two dates) and is commonly used for deadline management, SLA tracking, payment terms, and project scheduling. For organizations that don't follow a standard Monday-Friday schedule, use WORKDAY.INTL instead, which allows custom weekend definitions.

## Syntax

> [!f(x)] WORKDAY Syntax
>
> ```
> =WORKDAY(start_date, days, [holidays])
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `start_date` | Yes | The starting date from which to count working days. Can be a date value, cell reference, or date-returning function. |
| `days` | Yes | The number of working days to add (positive) or subtract (negative) from the start date. Use positive numbers to go forward in time, negative to go backward. |
| `holidays` | No | An optional range or array of dates to exclude as non-working days in addition to weekends. These can be company holidays, national holidays, or any other non-working dates. |

### Return Value

Returns a serial number representing a date that is the specified number of working days from the start date. The returned date will always be a weekday (Monday-Friday) that is not in the holidays list.

## Examples

> [!f(x)] WORKDAY Examples

### Example 1: Add 10 Working Days
```
=WORKDAY(DATE(2026, 1, 5), 10)
```
**Result:** 1/19/2026 (Monday)

**Explanation:** Starting from Monday January 5, 2026, counting 10 working days forward skips two weekends (Jan 10-11 and Jan 17-18), arriving at Monday January 19.

---

### Example 2: Calculate Delivery Date
```
=WORKDAY(A1, 5)
```
**Result:** Date 5 business days after the date in A1

**Explanation:** Ideal for shipping estimates—if an order ships on a Monday, 5 business days puts delivery on the following Monday.

---

### Example 3: Subtract Working Days (Negative Days)
```
=WORKDAY(DATE(2026, 1, 15), -5)
```
**Result:** 1/8/2026 (Thursday)

**Explanation:** Counts 5 working days backward from January 15, 2026. Use negative values to find past dates.

---

### Example 4: Include Holiday List
```
=WORKDAY(DATE(2026, 1, 2), 10, B2:B10)
```
**Result:** Date 10 working days from January 2, excluding holidays in B2:B10

**Explanation:** If B2:B10 contains holiday dates, WORKDAY skips those in addition to weekends. Essential for accurate business calendar calculations.

---

### Example 5: Payment Due Date (Net 30 Business Days)
```
=WORKDAY(invoice_date, 30, Holidays)
```
**Result:** Payment due date exactly 30 business days after invoice

**Explanation:** Common for B2B payment terms. "Net 30 business days" is precisely calculated, not the 42+ calendar days it might actually represent.

---

### Example 6: Project Deadline from Start
```
=WORKDAY(project_start, duration_in_days, company_holidays)
```
**Result:** Project completion date based on working days

**Explanation:** If a project requires 20 working days and starts January 5, WORKDAY calculates the actual calendar date accounting for weekends and holidays.

---

### Example 7: Next Business Day (0 Days)
```
=WORKDAY(TODAY(), 0)
```
**Result:** TODAY() if today is a weekday, otherwise the previous Friday

**Explanation:** Surprisingly, 0 days returns the start date if it's a workday, or backs up to the previous workday if it falls on a weekend. Use 1 for guaranteed next business day.

---

### Example 8: Handle Start Date on Weekend
```
=WORKDAY(DATE(2026, 1, 10), 5)
```
**Result:** 1/16/2026 (Friday)

**Explanation:** January 10, 2026 is a Saturday. WORKDAY moves to Monday (Jan 12) and then counts 5 working days, ending on Friday.

---

### Example 9: SLA Response Time
```
=WORKDAY(ticket_opened, 3, SLA_Holidays)
```
**Result:** SLA response deadline

**Explanation:** For a 3-business-day SLA, calculate the exact date/time responses are due. Include any SLA-specific non-working days.

---

### Example 10: Array of Holidays
```
=WORKDAY(A1, 10, {DATE(2026,12,25), DATE(2026,12,26), DATE(2027,1,1)})
```
**Result:** 10 working days from A1, excluding Christmas and New Year's

**Explanation:** Instead of a range reference, holidays can be specified directly as an array constant. Useful for quick calculations without a separate holiday table.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#VALUE!` | Invalid start_date (not a date) | Ensure start_date is a valid date value or parseable date text |
| `#VALUE!` | Days parameter is non-numeric | Verify days is a number (can be positive, negative, or zero) |
| `#VALUE!` | Holiday range contains non-date values | Ensure all cells in holidays range are dates or blank |
| `#NUM!` | Calculation would result in invalid date | Check that result date is within Excel's valid date range |
| Incorrect result | Missing holidays in list | Verify your holiday list is comprehensive and covers the date range |
| Wrong date | Start date on weekend confusion | Understand that weekend start dates shift the calculation start point |

## Use Cases

### [[Contract and Legal Deadline Management]]

**Scenario:** Calculate legally binding deadlines that are specified in business days, such as response periods, notice requirements, and filing deadlines.

**Implementation:**
```
=WORKDAY(notice_date, 30, court_holidays)              (30-day response deadline)
=WORKDAY(filing_date, 10, federal_holidays)            (10-day appeal period)
=IF(WORKDAY(A1, 5) < TODAY(), "Overdue", "Pending")    (Deadline status check)
```

**Business Application:** Law firms track filing deadlines accurately. Contract administrators calculate notice periods. Compliance teams ensure regulatory response windows are met.

**Technical Details:** Legal deadlines often have specific rules about how to count business days. Verify whether your jurisdiction counts the start date, includes partial days, or has specific holiday calendars required by statute.

---

### [[Shipping and Logistics Scheduling]]

**Scenario:** Calculate estimated delivery dates based on shipping lead times expressed in business days, coordinating carrier schedules with customer expectations.

**Implementation:**
```
=WORKDAY(ship_date, carrier_lead_time, carrier_holidays)
=IF(WORKDAY(order_date, processing_days) > promised_date, "Expedite", "Standard")
=WORKDAY(TODAY(), production_days + shipping_days, all_holidays)
```

**Business Application:** E-commerce platforms display accurate delivery estimates. Logistics coordinators plan warehouse staffing. Customer service sets realistic expectations for order fulfillment.

**Technical Details:** Different carriers may have different non-working days (especially for international shipping). Consider separate holiday lists for different shipping regions or carriers.

---

### [[Human Resources and Payroll Processing]]

**Scenario:** Calculate onboarding timelines, benefits eligibility dates, and payroll processing deadlines based on business days.

**Implementation:**
```
=WORKDAY(hire_date, 5, company_holidays)               (Orientation complete date)
=WORKDAY(period_end, 3, payroll_holidays)              (Payroll processing deadline)
=WORKDAY(termination_date, -14, company_holidays)      (Two-week notice start)
```

**Business Application:** HR teams schedule new hire activities. Payroll departments set processing cutoffs. Benefits administrators calculate eligibility windows.

**Technical Details:** Different departments may observe different holidays. Corporate holidays might differ from factory holidays. Build role-specific or location-specific holiday calendars.

## Platform Differences

### Microsoft Excel
- **Availability:** Excel 2007 and later; Analysis ToolPak add-in required in earlier versions
- **Weekend definition:** Always Saturday-Sunday (use WORKDAY.INTL for custom weekends)
- **Holiday handling:** Accepts range references or array constants
- **Maximum range:** Works with standard Excel date limitations

### Google Sheets
- **Availability:** All versions
- **Behavior:** Identical to Excel's WORKDAY
- **Weekend definition:** Saturday-Sunday only (use WORKDAY.INTL for alternatives)
- **Array support:** Full support for array holiday lists

### Key Difference Alert
WORKDAY is functionally identical across Excel and Sheets. The main consideration is ensuring your holiday list is complete and accurate. Both platforms assume a Saturday-Sunday weekend—for different weekend patterns (like Friday-Saturday in Middle Eastern countries), use WORKDAY.INTL.

## Tips and Best Practices

1. **Build a comprehensive holiday calendar:** Create a named range (e.g., "CompanyHolidays") containing all non-working dates for your organization. Update it annually. Include this range in all WORKDAY formulas.

2. **Verify start date handling:** If start_date falls on a weekend, WORKDAY effectively starts counting from the next weekday. Test your formulas with weekend start dates to confirm expected behavior.

3. **Use WORKDAY.INTL for non-standard weeks:** If your business operates on a different schedule (like Sunday-Thursday), WORKDAY only handles Saturday-Sunday weekends. Switch to WORKDAY.INTL for custom weekend patterns.

4. **Combine with NETWORKDAYS for verification:** To verify calculations, use NETWORKDAYS between your start date and WORKDAY result—it should equal your days parameter.

5. **Document holiday sources:** Note where your holiday list comes from (government calendar, corporate HR, union contract) so future users know how to maintain it.

6. **Handle cross-year calculations:** Ensure your holiday list covers the full date range you might calculate. A 60-business-day calculation starting in November might extend into the following year.

## Related Functions

### Similar Functions

| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[WORKDAY.INTL]] | Add working days with custom weekends | When weekends are not Saturday-Sunday |
| [[NETWORKDAYS]] | Count working days between dates | When you need to count days, not find an end date |
| [[EDATE]] | Add calendar months to a date | When calculating by months, not business days |

### Commonly Used Together

**[[NETWORKDAYS]]** - Verify calculations

*Cross-check WORKDAY results:*
```
=NETWORKDAYS(start, WORKDAY(start, 10, holidays), holidays)    // Should equal 10
=WORKDAY(A1, NETWORKDAYS(A1, B1, holidays), holidays)          // Should equal B1
```
NETWORKDAYS counts the same business days that WORKDAY skips.

---

**[[TODAY]]** - Dynamic deadline calculations

*Calculate from current date:*
```
=WORKDAY(TODAY(), 5, holidays)         // 5 business days from today
=WORKDAY(TODAY(), -10, holidays)       // 10 business days ago
```
Creates dynamic due dates that update automatically.

---

**[[IF]]** - Conditional deadline logic

*Apply business rules:*
```
=IF(priority = "Rush", WORKDAY(A1, 3, holidays), WORKDAY(A1, 10, holidays))
=IF(WORKDAY(start, deadline_days, holidays) > TODAY(), "On Track", "Overdue")
```
Different scenarios require different lead times.

---

**[[TEXT]]** - Format deadline dates

*Display formatted results:*
```
=TEXT(WORKDAY(A1, 10, holidays), "dddd, mmmm d, yyyy")
="Due: " & TEXT(WORKDAY(TODAY(), 5, holidays), "mm/dd")
```
Present deadline dates in readable formats.

## Official Documentation

- **Microsoft Excel:** [WORKDAY function](https://support.microsoft.com/en-us/office/workday-function-f764a5b7-05fc-4494-9486-60d494efbf33)
- **Google Sheets:** [WORKDAY function](https://support.google.com/docs/answer/3295011)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel 2007 | 2007 | Native function (previously required Analysis ToolPak) |
| Excel 2010+ | 2010 | WORKDAY.INTL added as enhanced alternative |
| Excel 365 | Current | No changes |
| Google Sheets | Original launch (2006) | Full support |
| LibreOffice Calc | All versions | Compatible implementation |

---

*Last updated: 2026-01-10*
