---
categories:
- spreadsheet-functions
subCategories:
- excel
- sheets
topics:
- information
subTopics:
- system-info
- environment
- configuration
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- System Information
- Environment Info
- OS Information
tags:
- information
- system
- environment
- operating-system
- directory
---

# INFO

## Description

**INFO** returns information about the current operating environment, including the operating system, Excel version, available memory, current directory, and recalculation mode. It's the spreadsheet equivalent of a system properties panel—providing runtime context that formulas can use to adapt their behavior based on the environment.

The function accepts a single text argument specifying what information you want, then returns that specific system property. This enables platform-aware formulas that can detect whether they're running on Windows or Mac, check available memory before heavy calculations, or verify the Excel version to ensure function compatibility. While not glamorous, INFO is essential for building robust, portable spreadsheets.

**Common use cases:** Documentation templates that display the creation environment, compatibility checks before using version-specific features, troubleshooting helpers that capture system state, and cross-platform spreadsheets that adapt behavior based on the operating system.

**Important limitation:** Google Sheets supports only a limited subset of INFO type_text values. While Excel offers about 8 different info types, Google Sheets supports only "directory" and "numfile"—and even these return different information than in Excel. For truly cross-platform spreadsheets, avoid relying heavily on INFO, or wrap calls in IFERROR for graceful degradation.

## Syntax

> [!f(x)] INFO Syntax
>
> ```
> =INFO(type_text)
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `type_text` | Yes | A text string specifying what environment information to return. Must be one of the predefined values listed below. Case-insensitive. |

### Type Text Values

| Type_text | Returns | Excel | Sheets |
|-----------|---------|-------|--------|
| `"directory"` | Current default directory path | Yes | Limited |
| `"numfile"` | Number of active worksheets in open workbooks | Yes | Limited |
| `"origin"` | Cell address of top-left visible cell as text (with $ prefix) | Yes | No |
| `"osversion"` | Operating system name and version | Yes | No |
| `"recalc"` | Current recalculation mode: "Automatic" or "Manual" | Yes | No |
| `"release"` | Excel version number as text | Yes | No |
| `"system"` | Operating system type: "pcdos" (Windows), "mac" (macOS) | Yes | No |
| `"memavail"` | Available memory in bytes (legacy, often returns 0 or large value) | Yes (legacy) | No |
| `"memused"` | Memory used by Excel (legacy, often unreliable) | Yes (legacy) | No |
| `"totmem"` | Total available memory (legacy, often unreliable) | Yes (legacy) | No |

### Return Value

Returns text or number depending on the type_text parameter. Most return text strings (directory, osversion, recalc, release, system). "numfile" returns a number. Memory-related types return numbers but may be unreliable in modern Excel.

## Examples

> [!f(x)] INFO Examples

### Example 1: Get Operating System Type
```
=INFO("system")
```
**Result:** "pcdos" on Windows, "mac" on macOS

**Explanation:** Returns a simple identifier for the operating system family. Despite the archaic "pcdos" label, this correctly identifies Windows versions. Useful for platform-specific logic.

---

### Example 2: Get Detailed OS Version
```
=INFO("osversion")
```
**Result:** "Windows (32-bit) NT 10.0" or "Macintosh 12.4" (example)

**Explanation:** Returns the full operating system name and version number. More detailed than "system"—includes Windows version number or macOS version. Useful for documentation and troubleshooting.

---

### Example 3: Get Excel Version
```
=INFO("release")
```
**Result:** "16.0" for Office 2016/2019/365, "15.0" for 2013, etc.

**Explanation:** Returns the major Excel version as a text string. Excel 2010 = 14.0, 2013 = 15.0, 2016/2019/365 = 16.0. Useful for version compatibility checks before using newer functions.

---

### Example 4: Get Current Directory
```
=INFO("directory")
```
**Result:** "C:\Users\Name\Documents\" (example path)

**Explanation:** Returns the default file directory. This is Excel's current working directory, which may differ from where the workbook is saved. Useful for file path construction.

---

### Example 5: Check Recalculation Mode
```
=INFO("recalc")
```
**Result:** "Automatic" or "Manual"

**Explanation:** Returns the current calculation mode setting. Useful for warning users if manual recalculation is enabled (formulas might be stale) or for documentation purposes.

---

### Example 6: Count Open Worksheets
```
=INFO("numfile")
```
**Result:** 5 (example—total worksheets across all open workbooks)

**Explanation:** Returns the total number of active worksheets in all currently open workbooks, not just the current workbook. Somewhat legacy functionality; rarely used in modern scenarios.

---

### Example 7: Get Top-Left Visible Cell
```
=INFO("origin")
```
**Result:** "$A$1" (or whatever cell is at the top-left of the view)

**Explanation:** Returns the address of the cell at the top-left corner of the visible window. Changes as you scroll. Useful for capturing view state or creating scroll-aware formulas.

---

### Example 8: Platform-Specific Path Separator
```
=IF(INFO("system")="mac", "/", "\")
```
**Result:** "/" on Mac, "\" on Windows

**Explanation:** Uses INFO("system") to determine the correct path separator for file path construction. Mac uses forward slashes, Windows uses backslashes.

---

### Example 9: Version Compatibility Check
```
=IF(VALUE(INFO("release"))>=16, "Supported", "Please upgrade Excel")
```
**Result:** "Supported" for Excel 2016+ users

**Explanation:** Converts the version string to a number and checks if it's at least 16.0. Useful for warning users before they try to use features from newer Excel versions.

---

### Example 10: Documentation Footer
```
="Generated in Excel " & INFO("release") & " on " & INFO("osversion") & " at " & TEXT(NOW(), "yyyy-mm-dd hh:mm")
```
**Result:** "Generated in Excel 16.0 on Windows (64-bit) NT 10.0 at 2026-01-10 14:30"

**Explanation:** Combines multiple INFO calls with timestamp to create a complete documentation string. Useful for audit trails, report footers, and version tracking.

---

### Example 11: Memory Check (Legacy)
```
=INFO("memavail")
```
**Result:** Large number or 0

**Explanation:** Historically returned available memory in bytes. In modern Excel, this often returns very large values or 0 as memory management has changed. Mostly legacy functionality; not recommended for actual memory-based decisions.

---

### Example 12: Cross-Platform Safe Formula
```
=IFERROR(INFO("osversion"), "Unknown OS")
```
**Result:** OS version on Excel, "Unknown OS" on Google Sheets

**Explanation:** Wraps INFO in IFERROR since Google Sheets doesn't support "osversion". Provides graceful fallback for cross-platform spreadsheets.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#VALUE!` | Invalid type_text string | Use only supported type_text values: "directory", "numfile", "origin", "osversion", "recalc", "release", "system". Check spelling. |
| `#NAME?` | Missing quotes around type_text | type_text must be in quotes: INFO("system") not INFO(system). |
| `#N/A` | Using unsupported type_text in Google Sheets | Sheets only supports limited type_text values. Wrap in IFERROR for graceful handling. |
| `Empty or unexpected result` | Platform-specific behavior | Some values differ between Windows/Mac. "memavail" and similar may return 0 or large values. |
| `Stale value` | INFO("recalc") shows wrong mode | May require recalculation to update. Usually accurate but can lag after settings change. |

## Use Cases

### [[Cross-Platform Spreadsheet Compatibility]]

**Scenario:** Create spreadsheets that work on both Windows and Mac, adapting file paths and behaviors automatically.

**Implementation:**
```
Path separator formula:
=IF(INFO("system")="mac", "/", "\")

Full path construction:
=INFO("directory") & IF(INFO("system")="mac", "/", "\") & "DataFile.xlsx"
```

**Business Application:** Templates shared between Mac and Windows users. Automated file export paths. Multi-platform business solutions.

**Technical Details:** INFO("system") returns "mac" on macOS and "pcdos" on Windows (all versions). Use this to switch between Mac forward slashes and Windows backslashes in path construction.

---

### [[Version-Aware Feature Usage]]

**Scenario:** Check Excel version before using newer functions, providing fallback for older versions.

**Implementation:**
```
=IF(VALUE(INFO("release"))>=16,
    TEXTJOIN(",",TRUE,A1:A10),
    CONCATENATE(A1,",",A2,",",A3,",",A4))
```

**Business Application:** Spreadsheets that must work across corporate environments with mixed Excel versions. Template distribution where recipients may have older software.

**Technical Details:** Excel version numbers: 14.0 = 2010, 15.0 = 2013, 16.0 = 2016/2019/365. Note that 2016, 2019, and 365 all show as 16.0, so you can't distinguish between them using INFO alone.

---

### [[Audit Trail Documentation]]

**Scenario:** Automatically document the environment when a spreadsheet is created or modified.

**Implementation:**
```
Cell A1: ="Document created: " & TEXT(NOW(),"yyyy-mm-dd")
Cell A2: ="Excel version: " & INFO("release")
Cell A3: ="Operating system: " & INFO("osversion")
Cell A4: ="Calculation mode: " & INFO("recalc")
```

**Business Application:** Financial models requiring audit trails. Regulatory compliance documentation. Template version tracking.

**Technical Details:** NOW() is volatile and updates constantly. For static creation dates, paste values instead of formulas, or use VBA to capture the date once when the file is created.

---

### [[Manual Calculation Warning]]

**Scenario:** Alert users when spreadsheet is in manual calculation mode to prevent working with stale data.

**Implementation:**
```
=IF(INFO("recalc")="Manual",
    "WARNING: Manual calculation mode - Press F9 to update",
    "Calculation mode: Automatic")
```

**Business Application:** Complex financial models often use manual calculation for performance. This ensures users know to refresh before making decisions.

**Technical Details:** Consider conditional formatting to make the warning visually prominent. Users can press F9 (calculate now), Shift+F9 (calculate current sheet), or Ctrl+Alt+F9 (full recalculation).

## Platform Differences

### Microsoft Excel

- **Full support:** All type_text values are supported
- **"system" returns:** "pcdos" for Windows, "mac" for macOS
- **"osversion" returns:** Detailed OS information including version number
- **"release" returns:** Excel version (14.0, 15.0, 16.0, etc.)
- **Memory functions:** Present but often unreliable in modern versions
- **Recalc detection:** Accurately returns "Automatic" or "Manual"

### Google Sheets

- **Limited support:** Only "directory" and "numfile" officially work
- **"directory" returns:** Often empty or different behavior than Excel
- **"numfile" returns:** May return different count than expected
- **Unsupported types:** "system", "osversion", "release", "recalc", "origin" return errors
- **Workaround:** Use IFERROR to handle unsupported type_text values

### Key Difference Alert

INFO is primarily an Excel function with very limited Google Sheets support. Most useful INFO types (system, osversion, release, recalc) are Excel-only. If your spreadsheets need to work in both platforms, avoid relying on INFO or always wrap calls in IFERROR with appropriate fallbacks.

## Tips and Best Practices

1. **Wrap in IFERROR for cross-platform use:** Since Google Sheets doesn't support most INFO types, use `=IFERROR(INFO("release"),"Unknown")` for graceful degradation.

2. **Don't rely on memory info:** The memory-related type_text values ("memavail", "memused", "totmem") are legacy features that often return inaccurate or meaningless values in modern Excel.

3. **Use for documentation, not critical logic:** INFO is great for audit trails and documentation. For critical application logic, consider more robust approaches or testing across environments.

4. **Remember INFO("release") groups versions:** Excel 2016, 2019, and 365 all return "16.0". You can't distinguish between them using INFO alone.

5. **Combine with other functions:** INFO works well with IF, CHOOSE, and text functions to build environment-aware formulas and documentation strings.

6. **Check recalc mode in complex workbooks:** If your workbook is set to manual calculation, prominently display this using INFO("recalc") so users know to refresh.

## Related Functions

### Similar Functions
| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[CELL]] | Returns cell-level properties | When you need cell properties rather than system info |
| [[GET.CELL]] | Extended cell properties (XLM) | When you need formatting info that CELL doesn't provide |
| [[TYPE]] | Returns value type number | When checking the data type of a value |
| [[NOW]] / [[TODAY]] | Current date/time | When you need current date/time rather than system info |

### Commonly Used Together

**[[IF]]** - Conditional logic based on system info

*Combined with INFO for platform-specific behavior:*
```
=IF(INFO("system")="mac", "Use Cmd+C", "Use Ctrl+C")
```
Display platform-appropriate instructions.

---

**[[VALUE]]** - Convert version string to number

*Combined with INFO for version comparisons:*
```
=IF(VALUE(INFO("release"))>=15, "Use CONCAT", "Use CONCATENATE")
```
Convert "16.0" to 16 for numeric comparison.

---

**[[IFERROR]]** - Handle unsupported INFO types

*Combined with INFO for cross-platform safety:*
```
=IFERROR(INFO("osversion"), "OS info not available")
```
Gracefully handle Google Sheets incompatibility.

---

**[[TEXT]]** - Format INFO results

*Combined with INFO for documentation:*
```
=TEXT(NOW(),"yyyy-mm-dd") & " - Excel " & INFO("release")
```
Create formatted documentation strings.

---

**[[CONCATENATE]] / [[&]]** - Build info strings

*Combined with INFO for complete environment documentation:*
```
=INFO("system") & " / " & INFO("release")
```
Combine multiple INFO calls into summary text.

## Official Documentation

- **Microsoft Excel:** [INFO function](https://support.microsoft.com/en-us/office/info-function-725f259a-0e4b-44b0-af8b-e4bc8bd1b00b)
- **Google Sheets:** [INFO function](https://support.google.com/docs/answer/3093154) (limited support)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel | Excel 2.0 (1987) | Original function with most type_text values |
| Excel 5.0+ | 1993+ | Continued support; memory values became less reliable |
| Excel 2007+ | All versions | No changes to functionality |
| Excel 365 | Current | Same behavior; still returns "16.0" for release |
| Google Sheets | Various | Very limited support; most type_text values return errors |

---

*Last updated: 2026-01-10*
