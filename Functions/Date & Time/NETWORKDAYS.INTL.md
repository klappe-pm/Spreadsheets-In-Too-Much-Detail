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
- international-workdays
- custom-weekend
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- International Network Days
- Custom Weekend Day Count
- Business Days International
tags:
- networkdays-intl
- business-days
- custom-weekend
- international
- weekend-pattern
---

# NETWORKDAYS.INTL

## Description

**NETWORKDAYS.INTL** calculates the number of working days between two dates with the ability to define custom weekend days. This enhanced version of NETWORKDAYS is essential for organizations that don't follow the traditional Saturday-Sunday weekend, enabling accurate business day counting for global operations, non-standard schedules, and 24/7 facilities.

The key advantage of NETWORKDAYS.INTL over standard NETWORKDAYS is the `weekend` parameter, which lets you specify exactly which days of the week should be treated as non-working days. This can be done using predefined weekend codes (1-17) representing common patterns, or a custom 7-character string where each character indicates whether that day is working (0) or non-working (1). The string format runs Monday through Sunday, providing complete flexibility.

**Weekend parameter explained:** The weekend argument works identically to WORKDAY.INTL. Use numeric codes 1-7 for common two-day weekend patterns (like code 7 for Friday-Saturday), codes 11-17 for single-day weekends (like code 11 for Sunday-only), or a 7-character string for any custom pattern. The string "0000011" means Saturday and Sunday are non-working, while "1000001" means Monday and Sunday are off. Using "0000000" treats all days as working days—useful for 24/7 operations where only specified holidays are non-working.

NETWORKDAYS.INTL is essential for multinational corporations operating across time zones and calendar systems, manufacturing facilities with non-standard shifts, and any business measuring performance or SLAs against custom work calendars. It pairs with WORKDAY.INTL—always use the same weekend parameter in both functions to ensure consistent calculations.

## Syntax

> [!f(x)] NETWORKDAYS.INTL Syntax
>
> ```
> =NETWORKDAYS.INTL(start_date, end_date, [weekend], [holidays])
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `start_date` | Yes | The starting date of the period. This date is included in the count if it's a working day. |
| `end_date` | Yes | The ending date of the period. This date is included in the count if it's a working day. |
| `weekend` | No | A number (1-17) indicating standard weekend patterns, or a 7-character string specifying which days are non-working. Default is 1 (Saturday-Sunday). |
| `holidays` | No | A range or array of dates to exclude as non-working days in addition to the defined weekend days. |

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

| Pattern | Meaning | Working Days Per Week |
|---------|---------|----------------------|
| "0000011" | Sat-Sun off | 5 days (standard) |
| "1000001" | Mon-Sun off | 5 days (Tue-Sat work) |
| "0000001" | Sunday off | 6 days |
| "0000000" | No weekend | 7 days (24/7 operation) |
| "1111100" | Mon-Fri off | 2 days (weekend operation) |
| "1110011" | Mon-Wed, Sat-Sun off | 2 days (Thu-Fri only) |

### Return Value

Returns an integer representing the count of working days between start_date and end_date, inclusive, using the specified weekend definition. Returns 0 if no working days exist in the range. Returns a negative number if end_date is before start_date.

## Examples

> [!f(x)] NETWORKDAYS.INTL Examples

### Example 1: Standard Weekend (Default)
```
=NETWORKDAYS.INTL(DATE(2026, 1, 5), DATE(2026, 1, 16))
```
**Result:** 10

**Explanation:** With default Saturday-Sunday weekend, identical to NETWORKDAYS. Counts Monday-Friday working days.

---

### Example 2: Middle Eastern Weekend (Friday-Saturday)
```
=NETWORKDAYS.INTL(DATE(2026, 1, 5), DATE(2026, 1, 16), 7)
```
**Result:** 10

**Explanation:** Weekend code 7 treats Friday-Saturday as the weekend. Working days are Sunday-Thursday.

---

### Example 3: Sunday-Only Weekend (6-Day Week)
```
=NETWORKDAYS.INTL(DATE(2026, 1, 5), DATE(2026, 1, 16), 11)
```
**Result:** 11

**Explanation:** With only Sunday as non-working, there are more working days in the same calendar period.

---

### Example 4: No Weekend (24/7 Operation)
```
=NETWORKDAYS.INTL(DATE(2026, 1, 5), DATE(2026, 1, 16), "0000000")
```
**Result:** 12

**Explanation:** All zeros mean every day is a working day. Only holidays (if specified) are excluded.

---

### Example 5: Custom String Pattern
```
=NETWORKDAYS.INTL(DATE(2026, 1, 5), DATE(2026, 1, 16), "1000001")
```
**Result:** 10

**Explanation:** Monday and Sunday are non-working (1s at positions 1 and 7). Working days are Tuesday-Saturday.

---

### Example 6: Compare Weekend Patterns
```
=NETWORKDAYS.INTL(A1, B1, 1) & " vs " & NETWORKDAYS.INTL(A1, B1, 7)
```
**Result:** Compares Western vs Middle Eastern business day counts

**Explanation:** Same date range may have different working day counts depending on weekend definition.

---

### Example 7: With Holidays
```
=NETWORKDAYS.INTL(DATE(2026, 12, 20), DATE(2027, 1, 5), 7, holiday_list)
```
**Result:** Working days in date range using Friday-Saturday weekend and excluding holidays

**Explanation:** Combines custom weekend with holiday exclusions for complete business calendar.

---

### Example 8: 4-Day Work Week
```
=NETWORKDAYS.INTL(DATE(2026, 1, 5), DATE(2026, 1, 30), "1110000")
```
**Result:** Working days with Monday-Wednesday off

**Explanation:** String "1110000" means Mon-Wed are non-working. Only Thu-Sun are working days—not a typical pattern but fully supported.

---

### Example 9: Verify WORKDAY.INTL Result
```
=NETWORKDAYS.INTL(A1, WORKDAY.INTL(A1, 10, 7), 7)
```
**Result:** 10

**Explanation:** NETWORKDAYS.INTL verifies WORKDAY.INTL calculations when using the same weekend parameter.

---

### Example 10: Dynamic Regional Calculation
```
=NETWORKDAYS.INTL(start, end, VLOOKUP(region, weekend_lookup, 2, FALSE), regional_holidays)
```
**Result:** Working days using region-specific weekend pattern

**Explanation:** Looks up the appropriate weekend code based on region, enabling one formula to work globally.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#VALUE!` | Invalid start_date or end_date | Ensure both are valid date values |
| `#VALUE!` | Invalid weekend string (not 7 characters) | Use exactly 7 characters of 0s and 1s |
| `#VALUE!` | Weekend string is all 1s ("1111111") | At least one day must be a working day |
| `#VALUE!` | Weekend string contains non-0/1 characters | Use only 0 (working) and 1 (non-working) |
| `#NUM!` | Invalid weekend number (e.g., 8, 9, 10) | Use codes 1-7 or 11-17 only |
| Inconsistent results | Different weekend parameter than WORKDAY.INTL | Ensure both functions use identical weekend parameters |

## Use Cases

### [[Multinational SLA Tracking]]

**Scenario:** Measure service level agreement compliance across regions with different weekend patterns, ensuring consistent performance metrics globally.

**Implementation:**
```
=NETWORKDAYS.INTL(opened, resolved, region_weekend, region_holidays)
=IF(NETWORKDAYS.INTL(opened, resolved, 7, UAE_holidays) > SLA, "Breach", "Met")
=AVERAGE(NETWORKDAYS.INTL(open_dates, close_dates, local_weekend, local_holidays))
```

**Business Application:** Global IT service desks measure ticket resolution against local business calendars. International customer support tracks response times by regional standards. Vendors demonstrate SLA compliance using customer's local calendar.

**Technical Details:** Build a lookup table mapping regions to weekend codes and holiday lists. Use VLOOKUP or INDEX/MATCH to dynamically select the right parameters for each region's calculations.

---

### [[Global Supply Chain Lead Time Calculation]]

**Scenario:** Calculate accurate lead times for shipments crossing multiple regions with different working day calendars.

**Implementation:**
```
=NETWORKDAYS.INTL(ship_date, delivery_date, destination_weekend, destination_holidays)
=NETWORKDAYS.INTL(order, manufacture_complete, factory_weekend) + NETWORKDAYS.INTL(ship, arrive, transit_weekend)
=WORKDAY.INTL(TODAY(), lead_time, supplier_weekend, supplier_holidays)
```

**Business Application:** Procurement teams calculate realistic delivery expectations. Logistics coordinators plan inventory based on actual working days in transit. Supply chain managers reconcile lead times across global supplier networks.

**Technical Details:** Different stages of the supply chain may operate on different calendars. A factory in China, freight forwarder in Singapore, and customer in Saudi Arabia each have different weekends. Track which calendar applies to which segment.

---

### [[Continuous Operation Performance Metrics]]

**Scenario:** Measure uptime, response times, and performance for 24/7 operations where traditional weekends don't apply.

**Implementation:**
```
=NETWORKDAYS.INTL(incident_start, incident_resolved, "0000000", planned_downtime)
=NETWORKDAYS.INTL(start, end, "0000000") * 24    (Total hours in operational period)
=1 - (downtime_days / NETWORKDAYS.INTL(period_start, period_end, "0000000"))
```

**Business Application:** Data center managers track uptime metrics. Hospital administrators measure bed availability. Emergency services calculate response coverage.

**Technical Details:** For 24/7 operations, use "0000000" as the weekend parameter—every day is a working day. Only dates explicitly listed in the holidays parameter (like planned maintenance) are excluded.

## Platform Differences

### Microsoft Excel
- **Availability:** Excel 2010 and later
- **Weekend codes:** Full support for 1-17 and 7-character strings
- **String format:** "0000011" (Monday=position 1, Sunday=position 7)
- **Not available:** Excel 2007 and earlier

### Google Sheets
- **Availability:** All versions
- **Behavior:** Identical to Excel 2010+
- **Weekend codes:** Same as Excel
- **Compatibility:** Full cross-platform support

### Key Difference Alert
Both platforms implement NETWORKDAYS.INTL identically. The only historical issue is Excel 2007 lacking the function—use NETWORKDAYS if you need to support older versions and can accept the Saturday-Sunday limitation.

## Tips and Best Practices

1. **Match weekend parameters across functions:** Always use the same weekend code/string in NETWORKDAYS.INTL and WORKDAY.INTL within the same workflow. Mismatched parameters cause inconsistent results.

2. **Use numeric codes when possible:** Codes 1-7 and 11-17 are more readable than string patterns. Reserve custom strings for unusual schedules not covered by the numeric codes.

3. **Create a regional calendar lookup:** Build a reference table with columns for region, weekend code, and holiday range name. Use this as a single source of truth for all regional calculations.

4. **Document custom string meanings:** If using patterns like "1000011", add a comment explaining "Monday and weekend off (Tue-Fri work week)". Future users will appreciate the clarity.

5. **Test edge cases:** Verify calculations when the start or end date falls on a non-working day. Understand that inclusive counting still applies—a working day at either endpoint is counted.

6. **Consider time zones:** For international operations, determine which time zone defines the "day". A Monday in Sydney is still Sunday in New York. Define clear rules for which calendar applies.

## Related Functions

### Similar Functions

| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[NETWORKDAYS]] | Count working days (Sat-Sun weekend only) | When you always use Saturday-Sunday weekend |
| [[WORKDAY.INTL]] | Add working days with custom weekends | When you need an end date, not a count |
| [[DAYS]] | Count all calendar days | When you need total days regardless of weekends |

### Commonly Used Together

**[[WORKDAY.INTL]]** - Complementary function

*Calculate and verify with matching weekends:*
```
=NETWORKDAYS.INTL(A1, WORKDAY.INTL(A1, 10, 7), 7)    // Should equal 10
=WORKDAY.INTL(A1, NETWORKDAYS.INTL(A1, B1, 7), 7)    // Should equal B1
```
Always use matching weekend parameters for consistent results.

---

**[[VLOOKUP]] / [[INDEX]]** - Dynamic parameter selection

*Look up weekend codes by region:*
```
=NETWORKDAYS.INTL(A1, B1, VLOOKUP(region, weekend_table, 2, FALSE))
=NETWORKDAYS.INTL(A1, B1, INDEX(weekends, MATCH(country, countries, 0)), INDIRECT(holiday_range))
```
Enable single formulas to work across multiple regions.

---

**[[IF]] / [[SWITCH]]** - Conditional logic

*Apply business rules based on counts:*
```
=IF(NETWORKDAYS.INTL(A1, TODAY(), 7) > 30, "Overdue", "Current")
=SWITCH(TRUE, NETWORKDAYS.INTL(A1, B1, weekend) <= 5, "Express", NETWORKDAYS.INTL(A1, B1, weekend) <= 15, "Standard", "Economy")
```
Categorize or flag based on working day counts.

---

**[[SUMIFS]] / [[AVERAGEIFS]]** - Aggregate with day criteria

*Analyze data filtered by working day ranges:*
```
=AVERAGEIFS(amounts, NETWORKDAYS.INTL(dates, TODAY(), 7), "<=30")
=SUMIFS(revenue, NETWORKDAYS.INTL(order_dates, ship_dates, "0000001"), ">=10")
```
Combine counting with aggregation for analysis.

## Official Documentation

- **Microsoft Excel:** [NETWORKDAYS.INTL function](https://support.microsoft.com/en-us/office/networkdays-intl-function-a9b26239-4f20-46a1-9571-d6830768e6c7)
- **Google Sheets:** [NETWORKDAYS.INTL function](https://support.google.com/docs/answer/3295893)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel 2010 | 2010 | First introduction with custom weekend support |
| Excel 2013+ | 2013 | No changes |
| Excel 365 | Current | No changes |
| Google Sheets | 2010 | Added alongside Excel compatibility |
| LibreOffice Calc | 4.0+ | Compatible implementation |

---

*Last updated: 2026-01-10*
