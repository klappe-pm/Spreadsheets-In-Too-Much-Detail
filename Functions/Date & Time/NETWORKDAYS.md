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
- day-counting
- workday-calculation
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Network Days
- Business Day Count
- Working Day Count
tags:
- networkdays
- business-days
- day-count
- workdays
- holidays
---

# NETWORKDAYS

## Description

**NETWORKDAYS** calculates the number of working days between two dates. Working days are defined as all days except Saturdays, Sundays, and any dates you specify as holidays. This function is essential for calculating project durations, measuring SLA compliance, aging analysis, and any business metric that needs to count business days rather than calendar days.

The function is inclusive of both the start and end dates—if both dates are workdays, they are both counted. For example, NETWORKDAYS from Monday to Tuesday returns 2 (both Monday and Tuesday are counted). This inclusive counting is important to understand because it differs from simple date subtraction, which gives the days between dates. The inclusive nature makes NETWORKDAYS perfect for counting "how many working days did this task take?"

**Understanding the count:** NETWORKDAYS answers "how many working days exist in this date range, including both endpoints?" If you start a task on Monday and complete it on Friday of the same week, NETWORKDAYS returns 5 (Mon, Tue, Wed, Thu, Fri). This is the actual number of working days the task occupied. The function pairs naturally with WORKDAY—if you add N working days to a date using WORKDAY, then NETWORKDAYS between the original and result date equals N.

NETWORKDAYS is widely used in HR for calculating vacation balances and tenure, in project management for measuring actual work time, in logistics for tracking lead times, and in finance for aging receivables and payables in business days. For organizations with non-standard weekends (like Friday-Saturday in the Middle East), use NETWORKDAYS.INTL instead.

## Syntax

> [!f(x)] NETWORKDAYS Syntax
>
> ```
> =NETWORKDAYS(start_date, end_date, [holidays])
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `start_date` | Yes | The starting date of the period. This date is included in the count if it's a working day. |
| `end_date` | Yes | The ending date of the period. This date is included in the count if it's a working day. |
| `holidays` | No | An optional range or array of dates to exclude as non-working days in addition to weekends. |

### Return Value

Returns an integer representing the count of working days between start_date and end_date, inclusive. Returns 0 if both dates fall on weekends or holidays with no working days between them. Returns a negative number if end_date is before start_date.

## Examples

> [!f(x)] NETWORKDAYS Examples

### Example 1: Basic Working Day Count
```
=NETWORKDAYS(DATE(2026, 1, 5), DATE(2026, 1, 9))
```
**Result:** 5

**Explanation:** Monday January 5 through Friday January 9 includes 5 working days. Both endpoints are counted.

---

### Example 2: Count Across a Weekend
```
=NETWORKDAYS(DATE(2026, 1, 5), DATE(2026, 1, 12))
```
**Result:** 6

**Explanation:** Monday to the following Monday spans 8 calendar days but only 6 working days (excludes Saturday and Sunday).

---

### Example 3: Same Day Start and End
```
=NETWORKDAYS(DATE(2026, 1, 5), DATE(2026, 1, 5))
```
**Result:** 1

**Explanation:** When start and end are the same working day, the result is 1 (that day is counted).

---

### Example 4: Weekend Start and End
```
=NETWORKDAYS(DATE(2026, 1, 10), DATE(2026, 1, 11))
```
**Result:** 0

**Explanation:** Saturday to Sunday contains no working days, so the result is 0.

---

### Example 5: Exclude Holidays
```
=NETWORKDAYS(DATE(2026, 12, 21), DATE(2027, 1, 2), B2:B5)
```
**Result:** Count excluding Christmas/New Year holidays in B2:B5

**Explanation:** If B2:B5 contains December 25, December 26, January 1, the function excludes those dates from the count along with weekends.

---

### Example 6: Project Duration in Working Days
```
=NETWORKDAYS(project_start, project_end, company_holidays)
```
**Result:** Actual working days the project took

**Explanation:** Measures real work time, not calendar time. A project spanning 30 calendar days might only be 20 working days.

---

### Example 7: Aging in Business Days
```
=NETWORKDAYS(invoice_date, TODAY(), holidays)
```
**Result:** Business days since invoice was issued

**Explanation:** For aging reports, counting business days gives a more accurate measure of payment delay than calendar days.

---

### Example 8: Negative Result (Reversed Dates)
```
=NETWORKDAYS(DATE(2026, 1, 15), DATE(2026, 1, 10))
```
**Result:** -4

**Explanation:** When end_date is before start_date, the result is negative. This can be useful for calculating "days until" scenarios.

---

### Example 9: Verify WORKDAY Calculation
```
=NETWORKDAYS(A1, WORKDAY(A1, 10, holidays), holidays)
```
**Result:** 10

**Explanation:** NETWORKDAYS can verify WORKDAY results. Adding 10 working days and then counting should return 10.

---

### Example 10: Days Until Deadline
```
=NETWORKDAYS(TODAY(), deadline_date, holidays)
```
**Result:** Working days remaining until deadline

**Explanation:** Shows how many actual work days remain. More actionable than calendar days for resource planning.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#VALUE!` | Invalid date in start_date or end_date | Ensure both parameters are valid dates |
| `#VALUE!` | Text that cannot be parsed as date | Use DATE function or valid date references |
| `#VALUE!` | Holiday range contains non-date values | Ensure all cells in holidays are dates or blank |
| Unexpected count | Forgetting inclusive counting | Remember both start and end dates are counted |
| Wrong result | Missing holidays from list | Ensure holiday list is comprehensive |
| Zero when expected positive | Both dates on weekends | Verify your date range includes working days |

## Use Cases

### [[Project Duration and Resource Planning]]

**Scenario:** Calculate actual working days projects take, compare planned versus actual durations, and plan future resource needs based on business day estimates.

**Implementation:**
```
=NETWORKDAYS(start_date, end_date, company_holidays)                    (Actual duration)
=NETWORKDAYS(planned_start, planned_end, holidays) - actual_days        (Variance)
=NETWORKDAYS(TODAY(), target_date, holidays)                            (Days remaining)
```

**Business Application:** Project managers compare estimated versus actual work effort. Resource planners allocate team capacity based on working days. Executives review project efficiency metrics normalized to business time.

**Technical Details:** Working day counts remove weekend noise from duration metrics. A task completed in 12 calendar days might be 8 working days—the 8-day figure better represents actual work time and is more comparable across projects.

---

### [[SLA Compliance Tracking]]

**Scenario:** Measure response and resolution times against Service Level Agreements specified in business days, flagging violations and calculating compliance percentages.

**Implementation:**
```
=IF(NETWORKDAYS(opened, resolved, holidays) > SLA_days, "Breach", "Met")
=NETWORKDAYS(opened, first_response, holidays)                          (Response time)
=COUNTIF(NETWORKDAYS(open_dates, close_dates, holidays), "<=" & SLA_days) / COUNT(tickets)
```

**Business Application:** IT help desks track ticket resolution SLAs. Customer service measures response time compliance. Vendors demonstrate contractual SLA adherence.

**Technical Details:** SLAs typically specify business days, not calendar days. "3-business-day response" means NETWORKDAYS from request to response should be 3 or less. Ensure your holiday list matches the SLA contract's holiday definition.

---

### [[Accounts Receivable and Payable Aging]]

**Scenario:** Age outstanding invoices and bills in business days for more accurate cash flow analysis and credit risk assessment.

**Implementation:**
```
=NETWORKDAYS(invoice_date, TODAY(), holidays)                           (Days outstanding)
=IF(NETWORKDAYS(due_date, TODAY(), holidays) > 0, "Overdue", "Current")
=SUMIFS(amounts, NETWORKDAYS(dates, TODAY()), ">30")                    (Over 30 business days)
```

**Business Application:** Credit analysts assess customer payment patterns. Treasury teams forecast cash timing. Collection teams prioritize accounts by business-day age.

**Technical Details:** Business day aging is more meaningful than calendar aging for B2B contexts where payments typically process on business days. An invoice aged 45 calendar days including a two-week holiday might only be 25 business days old.

## Platform Differences

### Microsoft Excel
- **Availability:** Excel 2007 and later (native); Analysis ToolPak add-in in earlier versions
- **Weekend definition:** Fixed Saturday-Sunday (use NETWORKDAYS.INTL for custom)
- **Inclusive counting:** Both start and end dates counted
- **Negative results:** Returns negative when end_date < start_date

### Google Sheets
- **Availability:** All versions
- **Behavior:** Identical to Excel
- **Weekend definition:** Saturday-Sunday only
- **Compatibility:** Full cross-platform compatibility

### Key Difference Alert
NETWORKDAYS behaves identically in Excel and Sheets. The key consideration is the inclusive counting of both endpoints, which can cause off-by-one confusion if users expect exclusive counting. Both platforms return negative values when dates are reversed, which can be useful or confusing depending on intent.

## Tips and Best Practices

1. **Remember inclusive counting:** NETWORKDAYS counts both start and end dates. Monday to Friday is 5 days, not 4. If you need exclusive counting (days between), subtract 1 from the result.

2. **Use named ranges for holidays:** Create a named range like "CompanyHolidays" and use it consistently in all NETWORKDAYS formulas. This ensures uniform calculations across your workbook.

3. **Pair with WORKDAY for validation:** If WORKDAY(A1, 10) = B1, then NETWORKDAYS(A1, B1) should equal 10. Use this relationship to verify your holiday lists are consistent.

4. **Handle reversed dates intentionally:** NETWORKDAYS returns negative when end < start. This can be useful for "days until" calculations but might cause unexpected results if dates are entered in wrong order.

5. **Use NETWORKDAYS.INTL for custom weekends:** If your organization doesn't follow Saturday-Sunday weekends, switch to NETWORKDAYS.INTL with the appropriate weekend parameter.

6. **Document your holiday sources:** Future users need to know where holiday dates come from and how to update them. Add notes about which calendar (country, company, union) applies.

## Related Functions

### Similar Functions

| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[NETWORKDAYS.INTL]] | Count working days with custom weekends | When weekends are not Saturday-Sunday |
| [[WORKDAY]] | Add working days to a date | When you need an end date, not a count |
| [[DAYS]] | Count all calendar days between dates | When you need total days, not working days |

### Commonly Used Together

**[[WORKDAY]]** - Complementary function

*Calculate and verify working day operations:*
```
=NETWORKDAYS(A1, WORKDAY(A1, 10, holidays), holidays)    // Always equals 10
=WORKDAY(A1, NETWORKDAYS(A1, B1, holidays), holidays)    // Always equals B1
```
WORKDAY and NETWORKDAYS are inverse operations.

---

**[[TODAY]]** - Dynamic date reference

*Count from/to current date:*
```
=NETWORKDAYS(start, TODAY(), holidays)          // Days elapsed
=NETWORKDAYS(TODAY(), deadline, holidays)       // Days remaining
```
Creates live-updating counts.

---

**[[IF]] / [[IFS]]** - Conditional categorization

*Categorize by business day age:*
```
=IF(NETWORKDAYS(A1, TODAY()) > 30, "Aged", "Current")
=IFS(NETWORKDAYS(A1, TODAY()) <= 5, "New", NETWORKDAYS(A1, TODAY()) <= 15, "Open", TRUE, "Aged")
```
Create aging buckets or status indicators.

---

**[[SUMIFS]] / [[COUNTIFS]]** - Aggregate by working day criteria

*Filter data by business day ranges:*
```
=COUNTIFS(NETWORKDAYS(dates, TODAY()), ">30")
=SUMIFS(amounts, NETWORKDAYS(invoice_dates, TODAY()), ">=15")
```
Analyze data by business day age.

## Official Documentation

- **Microsoft Excel:** [NETWORKDAYS function](https://support.microsoft.com/en-us/office/networkdays-function-48e717bf-a7a3-495f-969e-5005e3eb18e7)
- **Google Sheets:** [NETWORKDAYS function](https://support.google.com/docs/answer/3092979)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel 2007 | 2007 | Native function (previously Analysis ToolPak) |
| Excel 2010 | 2010 | NETWORKDAYS.INTL added as enhanced alternative |
| Excel 365 | Current | No changes |
| Google Sheets | Original launch (2006) | Full support |
| LibreOffice Calc | All versions | Compatible implementation |

---

*Last updated: 2026-01-10*
