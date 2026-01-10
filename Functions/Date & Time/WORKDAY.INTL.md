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
- international-workday
- custom-weekend
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- International Workday
- Custom Weekend Workday
- Business Day International
tags:
- workday-intl
- business-days
- custom-weekend
- international
- weekend-pattern
---

# WORKDAY.INTL

## Description

**WORKDAY.INTL** returns a date that is a specified number of working days before or after a starting date, with the ability to define custom weekend days. This enhanced version of WORKDAY is essential for organizations that don't follow the traditional Saturday-Sunday weekend, such as businesses in the Middle East (Friday-Saturday weekend), companies with unique schedules (Sunday-only rest day), or operations that run seven days a week with no weekends at all.

The key advantage of WORKDAY.INTL over standard WORKDAY is the `weekend` parameter, which allows you to specify exactly which days of the week should be treated as non-working days. This can be done using predefined weekend codes (1-17) or a custom 7-character string pattern where each character represents a day of the week. This flexibility makes WORKDAY.INTL suitable for any global operation or unconventional business schedule.

**Weekend parameter explained:** The weekend argument accepts either a number (1-17) representing common weekend patterns, or a 7-character string of 0s and 1s where 1 means non-working day. In the string format, the characters represent Monday through Sunday in order. For example, "0000011" means Saturday and Sunday are weekends (Monday-Friday work week), while "1000001" means Monday and Sunday are non-working days. This string format provides ultimate flexibility for any conceivable work schedule.

WORKDAY.INTL is commonly used in multinational corporations that operate across regions with different weekend patterns, manufacturing facilities with non-standard shifts, and any business that needs to calculate accurate lead times across different calendar systems. It pairs with NETWORKDAYS.INTL for counting business days under the same custom weekend definitions.

## Syntax

> [!f(x)] WORKDAY.INTL Syntax
>
> ```
> =WORKDAY.INTL(start_date, days, [weekend], [holidays])
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `start_date` | Yes | The starting date from which to count working days. |
| `days` | Yes | The number of working days to add (positive) or subtract (negative) from the start date. |
| `weekend` | No | A number (1-17) indicating standard weekend patterns, or a 7-character string specifying which days are non-working. Default is 1 (Saturday-Sunday weekend). |
| `holidays` | No | A range or array of dates to exclude as non-working days in addition to weekends. |

### Weekend Parameter Values

#### Numeric Weekend Codes

| Code | Weekend Days | Description |
|------|--------------|-------------|
| 1 or omitted | Saturday, Sunday | Standard Western weekend (default) |
| 2 | Sunday, Monday | |
| 3 | Monday, Tuesday | |
| 4 | Tuesday, Wednesday | |
| 5 | Wednesday, Thursday | |
| 6 | Thursday, Friday | |
| 7 | Friday, Saturday | Middle Eastern weekend (traditional) |
| 11 | Sunday only | 6-day work week, Sunday off |
| 12 | Monday only | 6-day work week, Monday off |
| 13 | Tuesday only | 6-day work week, Tuesday off |
| 14 | Wednesday only | 6-day work week, Wednesday off |
| 15 | Thursday only | 6-day work week, Thursday off |
| 16 | Friday only | 6-day work week, Friday off |
| 17 | Saturday only | 6-day work week, Saturday off |

#### Custom String Pattern

Use a 7-character string of 0s and 1s where:
- Position 1 = Monday, Position 2 = Tuesday, ... Position 7 = Sunday
- 0 = Working day, 1 = Non-working day

| Pattern | Meaning | Example Use Case |
|---------|---------|------------------|
| "0000011" | Sat-Sun weekend | Standard Monday-Friday business |
| "1000001" | Mon-Sun non-working | Tuesday-Saturday work week |
| "0000001" | Sunday only | 6-day week with Sunday off |
| "0000000" | No weekend | 7-day operation (only holidays off) |
| "1111100" | Mon-Fri non-working | Weekend-only operation |
| "1000011" | Mon, Sat, Sun off | 4-day work week (Tue-Fri) |

### Return Value

Returns a serial number representing a date that is the specified number of working days from the start date, respecting the custom weekend definition and excluding holidays.

## Examples

> [!f(x)] WORKDAY.INTL Examples

### Example 1: Standard Weekend (Default)
```
=WORKDAY.INTL(DATE(2026, 1, 5), 10)
```
**Result:** 1/19/2026

**Explanation:** With default weekend (Saturday-Sunday), this is identical to WORKDAY. Counts 10 working days from January 5, 2026.

---

### Example 2: Middle Eastern Weekend (Friday-Saturday)
```
=WORKDAY.INTL(DATE(2026, 1, 5), 10, 7)
```
**Result:** 1/18/2026

**Explanation:** Weekend code 7 treats Friday-Saturday as the weekend. Working days are Sunday-Thursday. Result differs from standard WORKDAY because different days are skipped.

---

### Example 3: Sunday-Only Weekend
```
=WORKDAY.INTL(DATE(2026, 1, 5), 10, 11)
```
**Result:** 1/16/2026

**Explanation:** Weekend code 11 means only Sunday is non-working. With a 6-day work week, 10 working days pass faster on the calendar.

---

### Example 4: Custom Weekend String (Mon-Sun Off)
```
=WORKDAY.INTL(DATE(2026, 1, 6), 10, "1000001")
```
**Result:** 1/19/2026

**Explanation:** The string "1000001" means Monday (position 1) and Sunday (position 7) are non-working. Work days are Tuesday-Saturday.

---

### Example 5: No Weekend (7-Day Operation)
```
=WORKDAY.INTL(DATE(2026, 1, 5), 10, "0000000")
```
**Result:** 1/15/2026

**Explanation:** All zeros mean no weekend days—every day is a working day. Only holidays (if specified) would be excluded.

---

### Example 6: Four-Day Work Week
```
=WORKDAY.INTL(DATE(2026, 1, 5), 10, "1110011")
```
**Result:** 1/22/2026

**Explanation:** Monday, Tuesday, Wednesday, Saturday, Sunday are non-working (1s). Only Thursday and Friday are working days—a 2-day work week means 10 working days spans 5 weeks.

---

### Example 7: UAE Weekend (Sat-Sun) with Holidays
```
=WORKDAY.INTL(DATE(2026, 1, 1), 20, 1, UAE_holidays)
```
**Result:** 20 working days from January 1, excluding UAE public holidays

**Explanation:** United Arab Emirates switched to Saturday-Sunday weekend in 2022. Previous Friday-Saturday weekend (code 7) is no longer used by most UAE businesses.

---

### Example 8: Retail Schedule (Weekday Closures)
```
=WORKDAY.INTL(DATE(2026, 1, 5), 5, 12)
```
**Result:** 5 business days, where Monday is the only day off

**Explanation:** Some retail businesses close Mondays. Weekend code 12 treats only Monday as non-working.

---

### Example 9: Compare Different Weekend Patterns
```
=WORKDAY.INTL(A1, 10, 1) & " vs " & WORKDAY.INTL(A1, 10, 7)
```
**Result:** Compares Western vs Middle Eastern deadline dates

**Explanation:** Shows how the same 10-business-day deadline falls on different calendar dates depending on weekend definition.

---

### Example 10: Dynamic Weekend Based on Region
```
=WORKDAY.INTL(A1, days, IF(region="ME", 7, 1), holidays)
```
**Result:** Uses appropriate weekend based on region code

**Explanation:** For multinational operations, select weekend pattern based on regional settings.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#VALUE!` | Invalid start_date | Ensure start_date is a valid date |
| `#VALUE!` | Invalid weekend string (not 7 characters, not 0/1) | Use exactly 7 characters, only 0s and 1s |
| `#VALUE!` | Weekend string is all 1s ("1111111") | At least one day must be a working day |
| `#NUM!` | Invalid weekend number (not 1-17) | Use valid weekend codes: 1-7, 11-17 |
| `#NUM!` | Result date out of range | Check calculation doesn't exceed Excel's date limits |
| Unexpected result | Wrong weekend pattern for intended schedule | Verify weekend parameter matches your actual work schedule |

## Use Cases

### [[Multinational Project Coordination]]

**Scenario:** Coordinate project deadlines across offices in different countries with varying weekend patterns—US (Sat-Sun), UAE (previously Fri-Sat, now Sat-Sun), Israel (Fri-Sat).

**Implementation:**
```
=WORKDAY.INTL(kickoff, duration, VLOOKUP(country, weekend_table, 2, FALSE), regional_holidays)
=MAX(WORKDAY.INTL(A1, 10, US_weekend), WORKDAY.INTL(A1, 10, UAE_weekend))    (Latest deadline)
=WORKDAY.INTL(global_start, days, local_weekend_code, local_holidays)
```

**Business Application:** Global IT projects set realistic go-live dates. International supply chains calculate transit times accurately. Consulting firms schedule client deliverables respecting local calendars.

**Technical Details:** Maintain a reference table mapping countries/regions to weekend codes. Some regions (like parts of India) may have Saturday as a half-day—WORKDAY.INTL can only handle full days, so you may need custom logic for partial working days.

---

### [[Manufacturing and Shift Operations]]

**Scenario:** Calculate production schedules for facilities operating on non-standard schedules—24/7 operations, 4-day work weeks, or rotating weekend patterns.

**Implementation:**
```
=WORKDAY.INTL(start, production_days, "0000000", shutdown_dates)    (24/7 facility)
=WORKDAY.INTL(order_date, lead_time, "1100001", factory_holidays)   (Tue-Sat factory)
=WORKDAY.INTL(maintenance_start, repair_days, shift_pattern, planned_outages)
```

**Business Application:** Factory managers calculate realistic delivery dates. Maintenance teams schedule equipment downtime. Production planners balance workload across operating days.

**Technical Details:** For 24/7 operations, use "0000000" as the weekend parameter—all days are working days, and only specified holidays halt production. Consider creating separate holiday lists for planned maintenance shutdowns.

---

### [[Religious and Cultural Calendar Compliance]]

**Scenario:** Schedule business activities respecting religious observances—Islamic Friday prayers, Jewish Shabbat, Christian Sunday services—where different teams or regions observe different rest days.

**Implementation:**
```
=WORKDAY.INTL(A1, 10, 7, Islamic_holidays)        (Islamic calendar: Fri-Sat off)
=WORKDAY.INTL(A1, 10, 17, Jewish_holidays)       (Jewish calendar: Saturday off)
=WORKDAY.INTL(A1, 10, IF(team="Orthodox", 11, 1), holidays)
```

**Business Application:** HR departments schedule training avoiding religious conflicts. Event planners choose dates accessible to all participants. Procurement timing respects supplier availability.

**Technical Details:** Religious holidays often follow lunar calendars and shift dates year-over-year. Maintain dynamic holiday lists that update based on religious calendar calculations.

## Platform Differences

### Microsoft Excel
- **Availability:** Excel 2010 and later
- **Weekend codes:** Full support for 1-17 and 7-character strings
- **String format:** "0000011" (Monday-Sunday, 0=work, 1=off)
- **Not in:** Excel 2007 and earlier (use WORKDAY with limitations)

### Google Sheets
- **Availability:** All versions
- **Weekend codes:** Same as Excel (1-17 and string patterns)
- **Behavior:** Identical to Excel 2010+
- **Compatibility:** Full cross-platform support

### Key Difference Alert
Both platforms handle WORKDAY.INTL identically. The main historical issue was Excel 2007 lacking this function entirely—if supporting legacy versions, fall back to WORKDAY or implement custom VBA solutions. String pattern format is identical across platforms.

## Tips and Best Practices

1. **Use weekend codes for common patterns:** Codes 1-7 and 11-17 are easier to read and less error-prone than string patterns. Reserve string patterns for truly custom schedules.

2. **Document your weekend pattern:** When sharing workbooks, clearly document which weekend pattern is in use. "0000011" is not immediately obvious—add a comment explaining "Sat-Sun weekend".

3. **Create a weekend lookup table:** For multinational organizations, build a reference table mapping countries/regions to weekend codes. Use VLOOKUP or INDEX/MATCH to select the appropriate code dynamically.

4. **Test with known dates:** Verify your weekend pattern by testing with dates where you know the expected result. For example, if Friday-Saturday is your weekend, WORKDAY.INTL of a Thursday + 1 day should return Sunday.

5. **Combine with NETWORKDAYS.INTL:** Use the same weekend parameter in both functions to ensure consistency when calculating and verifying business day counts.

6. **Handle transitions:** Some regions change their weekends (UAE changed from Fri-Sat to Sat-Sun in 2022). Consider date-based logic to apply different weekend patterns for historical vs. current calculations.

## Related Functions

### Similar Functions

| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[WORKDAY]] | Add working days (Sat-Sun weekend only) | When you always use Saturday-Sunday weekend |
| [[NETWORKDAYS.INTL]] | Count working days with custom weekends | When counting days between dates with custom weekends |
| [[EDATE]] | Add calendar months | When calculating by months, not business days |

### Commonly Used Together

**[[NETWORKDAYS.INTL]]** - Count with same weekend pattern

*Verify calculations use consistent definitions:*
```
=NETWORKDAYS.INTL(start, WORKDAY.INTL(start, 10, 7), 7)    // Should equal 10
=WORKDAY.INTL(A1, NETWORKDAYS.INTL(A1, B1, "0000001"), "0000001")
```
Always use the same weekend parameter in both functions.

---

**[[VLOOKUP]] / [[INDEX]]** - Dynamic weekend selection

*Look up weekend patterns by region:*
```
=WORKDAY.INTL(A1, 10, VLOOKUP(region, weekend_table, 2, FALSE), holidays)
=WORKDAY.INTL(A1, days, INDEX(weekends, MATCH(country, countries, 0)), holidays)
```
Enable automatic weekend pattern selection based on location or other criteria.

---

**[[IF]]** - Conditional weekend logic

*Apply different patterns conditionally:*
```
=WORKDAY.INTL(A1, 10, IF(YEAR(A1) < 2022, 7, 1), holidays)    // UAE transition
=IF(is_24x7, WORKDAY.INTL(A1, 10, "0000000"), WORKDAY.INTL(A1, 10, 1))
```
Handle complex business rules about which weekend pattern applies.

---

**[[TEXT]]** - Display formatted results

*Format the resulting date:*
```
=TEXT(WORKDAY.INTL(A1, 10, 7), "dddd, mmmm d")
="Deadline: " & TEXT(WORKDAY.INTL(TODAY(), 5, "0000011"), "yyyy-mm-dd")
```
Present results in user-friendly formats.

## Official Documentation

- **Microsoft Excel:** [WORKDAY.INTL function](https://support.microsoft.com/en-us/office/workday-intl-function-a378391c-9ba7-4678-8a39-39611a9bf81d)
- **Google Sheets:** [WORKDAY.INTL function](https://support.google.com/docs/answer/3295012)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel 2010 | 2010 | First introduction with custom weekend support |
| Excel 2013+ | 2013 | No changes to function |
| Excel 365 | Current | No changes |
| Google Sheets | 2010 | Added alongside Excel compatibility |
| LibreOffice Calc | 4.0+ | Compatible implementation |

---

*Last updated: 2026-01-10*
