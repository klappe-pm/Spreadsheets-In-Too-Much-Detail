---
categories:
- spreadsheet-functions
subCategories:
- excel
topics:
- information-functions
- system-information
subTopics:
- environment-info
- system-properties
- application-metadata
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- System Information
- Environment Info
- Application Info
tags:
- excel-only
- information
- system
- environment
---

# INFO

## Description

**INFO** returns information about the current operating environment, including operating system version, Excel version, available memory, and other system-level properties. Unlike CELL which queries individual cell properties, INFO queries the application and system environment as a whole. It answers questions like "What version of Excel is running?" and "What operating system is this?"

The function takes a single text argument specifying what information type to return. There are about a dozen info_types covering system details, directory paths, calculation modes, and memory statistics. INFO is particularly useful for creating adaptive workbooks that behave differently based on the environment, or for debugging purposes when a spreadsheet behaves unexpectedly.

**Important limitation:** Many INFO types are outdated legacy options from the DOS/early Windows era. Types like "memavail" and "totmem" return 0 or meaningless values in modern Excel. Also, results can vary significantly between Windows and Mac Excel, and some info_types don't exist on Mac.

**Platform note:** INFO is primarily an Excel function. While Google Sheets recognizes the function name, it only returns #N/A for all info_types since Google Sheets doesn't have equivalent system information to report. For practical purposes, INFO is Excel-only.

## Syntax

> [!f(x)] INFO Syntax
>
> ```
> =INFO(info_type)
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `info_type` | Yes | A text value specifying what type of system information to return. Must be one of the predefined info_type strings. |

### Info_Type Values

| Info_Type | Returns |
|-----------|---------|
| `"directory"` | Path of the current directory or folder |
| `"numfile"` | Number of active worksheets in open workbooks |
| `"origin"` | Absolute cell reference of top-left cell visible in window (for Lotus 1-2-3 compatibility) |
| `"osversion"` | Current operating system version as text |
| `"recalc"` | Current recalculation mode: "Automatic" or "Manual" |
| `"release"` | Version of Excel as text |
| `"system"` | Name of operating environment: "pcdos" (Windows) or "mac" (Macintosh) |
| `"memavail"` | Available memory in bytes (returns 0 in modern Excel) |
| `"memused"` | Memory used by data (returns 0 in modern Excel) |
| `"totmem"` | Total available memory including used (returns 0 in modern Excel) |

### Return Value

Returns a text string or number depending on the info_type requested.

## Examples

> [!f(x)] INFO Examples

### Example 1: Get Operating System Version
```
=INFO("osversion")
```
**Result:** `Windows (32-bit) NT 10.00` (on Windows 10/11)

**Explanation:** Returns the OS version string. Useful for documenting which system generated a report or for conditional logic based on operating system.

---

### Example 2: Get Excel Version
```
=INFO("release")
```
**Result:** `16.0` (for Excel 2016/2019/365)

**Explanation:** Returns the Excel version number. Version 16.0 indicates Excel 2016, 2019, 2021, or 365. Earlier versions return different numbers (15.0 for 2013, 14.0 for 2010).

---

### Example 3: Check Operating System Type
```
=INFO("system")
```
**Result:** `pcdos` (on Windows) or `mac` (on Mac)

**Explanation:** Returns a simple identifier for the operating system family. Despite the "pcdos" name (a legacy term), this indicates any Windows version.

---

### Example 4: Get Current Directory
```
=INFO("directory")
```
**Result:** `C:\Users\Username\Documents\`

**Explanation:** Returns the current working directory path. This is typically the folder from which Excel was launched or where the current file is saved.

---

### Example 5: Check Recalculation Mode
```
=INFO("recalc")
```
**Result:** `Automatic` or `Manual`

**Explanation:** Tells you whether Excel is set to automatic or manual calculation mode. Useful for troubleshooting when formulas don't seem to update.

---

### Example 6: Count Open Worksheets
```
=INFO("numfile")
```
**Result:** `5` (if 5 worksheets are active across all open workbooks)

**Explanation:** Counts all worksheets in all open workbooks, not just the current workbook. Can indicate workload or complexity of the current session.

---

### Example 7: Cross-Platform Detection
```
=IF(INFO("system")="mac", "Mac Version", "Windows Version")
```
**Result:** Platform-specific text

**Explanation:** Conditional formula that adapts based on operating system. Use this pattern to provide different instructions or use platform-specific features.

---

### Example 8: Version-Specific Feature Warning
```
=IF(VALUE(LEFT(INFO("release"),2))<16, "Upgrade recommended for full features", "Current version supported")
```
**Result:** Warning for older Excel versions

**Explanation:** Checks if Excel version is below 16 (pre-2016) and warns users. Useful when distributing workbooks that use newer functions.

---

### Example 9: Document System Environment
```
="Generated on " & INFO("osversion") & " using Excel " & INFO("release")
```
**Result:** `Generated on Windows (32-bit) NT 10.00 using Excel 16.0`

**Explanation:** Creates a system documentation string. Useful for audit trails, bug reports, or debugging information.

---

### Example 10: Get Scroll Position Origin
```
=INFO("origin")
```
**Result:** `$A$1` (typically, depends on scroll position)

**Explanation:** Returns the cell reference of the top-left visible cell. A legacy Lotus 1-2-3 compatibility feature, rarely needed in modern use.

---

### Example 11: Calculation Mode Check with Action
```
=IF(INFO("recalc")="Manual", "WARNING: Manual calculation enabled - press F9 to update", "Calculations are automatic")
```
**Result:** Warning message if manual calculation is on

**Explanation:** Alerts users when manual calculation mode might cause stale data. Important for complex workbooks where calculation mode might be changed.

---

### Example 12: Conditional Formatting Based on Platform
```
=IF(INFO("system")="mac", FORMULATEXT(A1), "Use Ctrl+` to see formulas")
```
**Result:** Platform-appropriate instructions

**Explanation:** Mac and Windows have different keyboard shortcuts. This pattern provides appropriate guidance based on platform.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#VALUE!` | Invalid info_type string | Use exact info_type strings from the list |
| `#NAME?` | Info_type not in quotes | Enclose info_type in quotation marks |
| Returns 0 | Using deprecated memory info_types | memavail, memused, totmem return 0 in modern Excel |
| `#N/A` | Running in Google Sheets | INFO doesn't work in Google Sheets |
| Unexpected format | Different results on Mac vs Windows | Test on both platforms if cross-platform use is expected |

## Use Cases

### [[Cross-Platform Workbook Compatibility]]

**Scenario:** Create workbooks that adapt their behavior or instructions based on the user's operating system.

**Implementation:**
```
=IF(INFO("system")="mac",
    "Press Cmd+Shift+L to apply filter",
    "Press Ctrl+Shift+L to apply filter")
```

**Business Application:** Training materials and templates distributed to both Windows and Mac users can provide platform-appropriate instructions without maintaining separate versions.

**Technical Details:** The "system" info_type reliably distinguishes Mac from Windows. Use this as the basis for any platform-conditional logic.

---

### [[Version Compatibility Checking]]

**Scenario:** Warn users when opening a workbook in an Excel version that doesn't support all features.

**Implementation:**
```
=IF(VALUE(LEFT(INFO("release"),2))<16,
    "This workbook uses features not available in your Excel version. Results may be incorrect.",
    "")
```

**Business Application:** Complex workbooks using newer functions (XLOOKUP, dynamic arrays, etc.) can self-document compatibility and alert users to potential issues.

**Technical Details:** Parse the version number from INFO("release") and compare to known feature introduction versions. Version 16.0 = Excel 2016+.

---

### [[Audit Trail Documentation]]

**Scenario:** Automatically document the environment where reports were generated.

**Implementation:**
```
="Report generated: " & TEXT(NOW(), "yyyy-mm-dd hh:mm") &
" | System: " & INFO("osversion") &
" | Excel: " & INFO("release") &
" | File: " & CELL("filename", A1)
```

**Business Application:** Financial reports and compliance documents can include system metadata for audit purposes, documenting exactly where and how the report was produced.

**Technical Details:** Combine INFO with CELL and date functions for comprehensive documentation. This creates defensible audit trails.

## Platform Differences

### Microsoft Excel (Windows)

| Info_Type | Support |
|-----------|---------|
| "directory" | Full support |
| "numfile" | Full support |
| "origin" | Full support |
| "osversion" | Returns Windows version string |
| "recalc" | Full support |
| "release" | Returns Excel version |
| "system" | Returns "pcdos" |
| Memory types | Return 0 (deprecated) |

### Microsoft Excel (Mac)

| Info_Type | Support |
|-----------|---------|
| "directory" | Full support (Mac path format) |
| "numfile" | Full support |
| "origin" | Full support |
| "osversion" | Returns Mac OS version string |
| "recalc" | Full support |
| "release" | Returns Excel version |
| "system" | Returns "mac" |
| Memory types | Return 0 (deprecated) |

### Google Sheets

| Info_Type | Support |
|-----------|---------|
| All info_types | Returns #N/A |

**Google Sheets does not support INFO functionality.** There is no equivalent function or workaround.

### Other Platforms

- LibreOffice Calc: Supports INFO with similar functionality
- Apple Numbers: INFO function not available

## Tips and Best Practices

1. **Use for documentation, not critical logic:** INFO is great for audit trails and user guidance, but avoid making critical calculations depend on it.

2. **Test cross-platform:** If workbooks will be used on both Windows and Mac, test INFO results on both platforms as formats differ.

3. **Ignore deprecated memory info_types:** memavail, memused, and totmem are vestigial from DOS-era Excel and return 0 in modern versions.

4. **Combine with conditional formatting:** Use INFO in conditional formatting rules to highlight version-specific warnings.

5. **Consider alternatives for version checking:** For more detailed version information, VBA's Application.Version property provides more granular data.

6. **Document INFO usage:** When using INFO for compatibility checks, document the logic clearly so future maintainers understand why.

## Related Functions

### Similar Functions
| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[CELL]] | Returns information about a specific cell | For cell-level properties like format or address |
| [[TYPE]] | Returns numeric type code of a value | For checking value data types |
| [[SHEET]] | Returns sheet number of a reference | For sheet position information |

### Commonly Used Together

**[[CELL]]** - Cell-level information

*Combine for comprehensive documentation:*
```
="File: " & CELL("filename", A1) & " | System: " & INFO("osversion")
```
CELL provides file-level info, INFO provides system-level info.

---

**[[IF]]** - Conditional logic based on environment

*Platform-adaptive formulas:*
```
=IF(INFO("system")="mac", MacFormula, WindowsFormula)
```
Build formulas that adapt to the runtime environment.

---

**[[VALUE]] / [[LEFT]]** - Parse version numbers

*Extract numeric version:*
```
=VALUE(LEFT(INFO("release"), 2))
```
Convert text version to number for comparison operations.

---

**[[NOW]]** - Timestamp documentation

*Full audit stamp:*
```
="Created: " & TEXT(NOW(),"yyyy-mm-dd") & " on " & INFO("osversion")
```
Combine timestamp with system info for complete documentation.

## Official Documentation

- **Microsoft Excel:** [INFO function](https://support.microsoft.com/en-us/office/info-function-725f259a-0e4b-49b3-8b52-58815c69acae)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel | Excel 1.0 | Original function from earliest versions |
| Excel 2007+ | Continued support | Memory info_types became deprecated |
| Excel 365 | Current | No significant changes |
| Google Sheets | N/A | Not functionally supported |

---

*Last updated: 2026-01-10*
