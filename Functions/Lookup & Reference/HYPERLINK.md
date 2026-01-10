---
categories:
- spreadsheet-functions
subCategories:
- excel
- sheets
topics:
- lookup-reference
subTopics:
- navigation
- web-links
- document-links
- interactive-spreadsheets
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Link Function
- URL Function
- Create Hyperlink
- Web Link
tags:
- lookup
- reference
- hyperlink
- navigation
- link
- url
---

# HYPERLINK

## Description

**HYPERLINK** creates a clickable link in a cell that can open a webpage, navigate to another location in the workbook, open another file, or trigger an email. Unlike manually inserted hyperlinks (via Insert > Link), HYPERLINK as a function can be constructed dynamically from cell values, making it powerful for generating reports with clickable references, building navigation systems, or creating data-driven link lists.

The function has two parameters: the link destination (URL, file path, or cell reference) and the optional friendly text to display. `=HYPERLINK("https://example.com", "Click here")` displays "Click here" as a clickable link. Without the friendly name, the full URL displays instead. The display text can be any string or formula result, enabling dynamic labels like employee names, product codes, or descriptive text.

**HYPERLINK supports multiple destination types:** Web URLs (`https://...`), local files (`C:\folder\file.xlsx`), network paths (`\\server\share\file.docx`), email addresses (`mailto:user@domain.com`), and internal worksheet locations (`#Sheet2!A1`). The internal navigation capability is particularly valuable for creating dashboards with "jump to" buttons, table of contents sheets, or back-to-top links in long reports.

**Security considerations matter.** HYPERLINK can potentially link to malicious websites or trigger unwanted downloads. In enterprise environments, be cautious about workbooks from untrusted sources that contain HYPERLINK formulas. Modern Excel and Sheets warn users before opening external links, but the risk exists. For internal applications, HYPERLINK is safe and extremely useful for navigation and integration.

## Syntax

> [!f(x)] HYPERLINK Syntax
>
> ```
> =HYPERLINK(link_location, [friendly_name])
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `link_location` | Yes | The URL, file path, or cell reference to link to. Must be text (in quotes) or a cell reference containing text. For internal links, use #SheetName!CellRef format. |
| `friendly_name` | No | The text to display in the cell. If omitted, link_location is displayed. Can be text, number, cell reference, or formula result. |

### Return Value

Returns a clickable hyperlink. The cell displays the friendly_name (or link_location if no friendly_name provided) and when clicked, navigates to the specified destination. The function itself returns the friendly_name value for use in other formulas.

## Examples

> [!f(x)] HYPERLINK Examples

### Example 1: Basic Web Link
```
=HYPERLINK("https://www.microsoft.com", "Microsoft Homepage")
```
**Result:** Displays "Microsoft Homepage" as a clickable link to Microsoft's website

**Explanation:** The most common use—creating a clickable link to a website. The URL must be complete with protocol (https:// or http://). The friendly name makes the link readable.

---

### Example 2: Web Link from Cell Reference
```
=HYPERLINK(A2, B2)
```
**Result:** Creates a link to the URL in A2, displaying the text from B2

**Explanation:** Dynamic linking where URLs and display text come from data. If A2 contains "https://google.com" and B2 contains "Search Engine", the cell shows "Search Engine" linking to Google.

---

### Example 3: Internal Workbook Navigation
```
=HYPERLINK("#Sheet2!A1", "Go to Sheet2")
```
**Result:** Clicking navigates to cell A1 on Sheet2

**Explanation:** The # prefix indicates an internal workbook link. This is powerful for creating navigation menus, table of contents, or "jump to" buttons in complex workbooks.

---

### Example 4: Navigate to Named Range
```
=HYPERLINK("#DataTable", "Jump to Data")
```
**Result:** Clicking navigates to the named range "DataTable"

**Explanation:** Named ranges work as link destinations. This is cleaner than cell references and survives structural changes. Define a named range and link to it with #NamedRange.

---

### Example 5: Email Link (Mailto)
```
=HYPERLINK("mailto:support@company.com?subject=Help Request", "Contact Support")
```
**Result:** Clicking opens email client with pre-filled address and subject

**Explanation:** The mailto: protocol opens the default email application. You can include subject and body parameters: `mailto:x@y.com?subject=Text&body=Message`. Useful for support contact lists.

---

### Example 6: Link to Local File
```
=HYPERLINK("C:\Reports\Q4Report.xlsx", "Open Q4 Report")
```
**Result:** Clicking opens the specified file

**Explanation:** Local file paths open files in their associated applications. Excel files open in Excel, PDFs in PDF reader, etc. Paths must be accessible from the user's machine.

---

### Example 7: Link to Network Share
```
=HYPERLINK("\\Server\SharedDocs\Budget.xlsx", "Budget Spreadsheet")
```
**Result:** Opens file from network share when clicked

**Explanation:** UNC paths (\\Server\Share\) work for network resources. Users must have appropriate network permissions. Useful for shared document repositories.

---

### Example 8: Dynamic URL Construction
```
=HYPERLINK("https://www.google.com/search?q="&ENCODEURL(A2), "Search: "&A2)
```
**Result:** Creates a Google search link for whatever text is in A2

**Explanation:** Build URLs dynamically by concatenating strings. ENCODEURL ensures special characters are properly escaped. Great for creating search links, map links, or API calls.

---

### Example 9: Link to Specific Cell with Row Reference
```
=HYPERLINK("#Details!A"&ROW(), "View Details")
```
**Result:** Links to the corresponding row on the Details sheet

**Explanation:** Use ROW() to create row-specific links. In row 5, this links to Details!A5. When you copy the formula down, each row links to its corresponding row on the other sheet.

---

### Example 10: Conditional Link Display
```
=IF(A2<>"", HYPERLINK("https://example.com/item/"&A2, "View Item"), "")
```
**Result:** Shows clickable link only if A2 has a value

**Explanation:** Wrap HYPERLINK in IF to create conditional links. This prevents broken links when source data is empty. The cell shows nothing when A2 is blank.

---

### Example 11: Table of Contents Navigation
```
=HYPERLINK("#'"&A2&"'!A1", A2)
```
**Result:** Creates a link to the sheet named in A2

**Explanation:** Dynamic sheet navigation. If A2 contains "January", this links to the January sheet. The single quotes handle sheet names with spaces. Perfect for auto-generated tables of contents.

---

### Example 12: Phone Number Link (Mobile/Touch)
```
=HYPERLINK("tel:+1-555-123-4567", "Call Sales")
```
**Result:** On mobile devices, clicking initiates a phone call

**Explanation:** The tel: protocol works on mobile devices and some desktop applications. Useful for contact lists viewed on tablets or phones.

---

### Example 13: Link with VLOOKUP Display Text
```
=HYPERLINK(B2, VLOOKUP(A2, ProductTable, 2, FALSE))
```
**Result:** Links to URL in B2, displays product name looked up from A2

**Explanation:** Combine HYPERLINK with lookup functions for rich, data-driven links. The display text comes from a lookup, while the URL comes from another column.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#VALUE!` | link_location is empty or invalid | Ensure the URL/path is a valid text string. Check for empty cells if using references. |
| `Link doesn't work` | Missing protocol (https://) or incorrect path | Always include full protocol for web links. Verify file paths exist and are accessible. |
| `#REF!` | Internal link references deleted sheet/range | Update the reference to a valid sheet or named range. |
| `Displays URL instead of name` | friendly_name omitted or evaluates to empty | Provide a friendly_name parameter, or ensure the cell/formula for it isn't empty. |
| `Security warning` | Excel/Sheets protecting against external links | This is expected behavior for security. Users must confirm navigation to external URLs. |
| `File not found` | Local file path is wrong or file moved | Verify the file exists at the specified path. Use relative paths where possible. |

## Use Cases

### [[Interactive Dashboard Navigation]]

**Scenario:** Create a dashboard with clickable buttons that navigate to different sheets, sections, or detail views within the same workbook—without using VBA or macros.

**Implementation:**
```
Table of Contents:
=HYPERLINK("#'"&A2&"'!A1", "Go to "&A2)

Back to Home button (on each sheet):
=HYPERLINK("#Home!A1", "← Back to Dashboard")

Jump to specific section:
=HYPERLINK("#ReportSection", "View Full Report")
```

**Business Application:** Executive dashboards with drill-down navigation, report workbooks with table of contents, and any multi-sheet workbook where users need easy navigation between sections.

**Technical Details:** Use named ranges for stable links that survive row/column changes. The # prefix is essential for internal links. Sheet names with spaces require single quotes: `#'Sheet Name'!A1`.

---

### [[Data-Driven Link Generation]]

**Scenario:** Generate clickable links dynamically based on data—like linking product codes to product pages, employee IDs to HR records, or order numbers to tracking URLs.

**Implementation:**
```
Product page links:
=HYPERLINK("https://store.com/product/"&ProductID, ProductName)

Employee profile links:
=HYPERLINK("https://hr.company.com/employee/"&EmployeeID, FullName)

Shipping tracker:
=HYPERLINK("https://track.fedex.com/track/"&TrackingNumber, "Track Package")

Google Maps link from address:
=HYPERLINK("https://maps.google.com/?q="&ENCODEURL(Address), "View Map")
```

**Business Application:** Product catalogs with clickable items, employee directories with profile links, order management systems with tracking links, and any list where items should link to detail pages.

**Technical Details:** Use ENCODEURL for addresses or search terms with spaces/special characters. Test URL patterns thoroughly—different websites have different URL structures for detail pages.

---

### [[Email Distribution List with Links]]

**Scenario:** Create contact lists where clicking a name opens an email with pre-filled recipient, subject line, and sometimes body text.

**Implementation:**
```
Basic email link:
=HYPERLINK("mailto:"&EmailAddress, PersonName)

With subject line:
=HYPERLINK("mailto:"&Email&"?subject=Meeting Request", "Email "&Name)

With subject and body:
=HYPERLINK("mailto:"&Email&"?subject="&ENCODEURL(Subject)&"&body="&ENCODEURL(BodyText), Name)

Multiple recipients:
=HYPERLINK("mailto:"&Email1&","&Email2&"?subject=Team Update", "Email Team")
```

**Business Application:** Employee directories, customer contact lists, support escalation sheets, and any scenario where clicking should initiate an email with context-appropriate subject or content.

**Technical Details:** Use `?subject=` and `&body=` for pre-filled content. ENCODEURL handles special characters in subjects and bodies. Multiple recipients are comma-separated after mailto:.

---

### [[Document Repository Index]]

**Scenario:** Create an index of documents stored on a file server or SharePoint, with clickable links to open each document directly.

**Implementation:**
```
Local file link:
=HYPERLINK(FilePath, FileName)

SharePoint/OneDrive link:
=HYPERLINK("https://company.sharepoint.com/sites/docs/"&FolderPath&"/"&FileName, FileName)

With file type icon (display):
=HYPERLINK(FilePath, FileType&": "&FileName)
```

**Business Application:** Document management indexes, project file directories, policy document libraries, and any structured document repository that needs a spreadsheet-based front end.

**Technical Details:** For network files, use UNC paths (\\Server\Share). For SharePoint, use web URLs. Consider relative paths for portable workbooks. Users need access permissions to linked files.

## Platform Differences

### Microsoft Excel

- **Availability:** All versions
- **Internal links:** Use #SheetName!Cell or #NamedRange
- **File links:** Full support for local and network paths
- **Email links:** Full mailto: support with subject and body
- **Security:** Warns before opening external links (configurable)
- **Display:** Shows as blue underlined text by default

### Google Sheets

- **Availability:** All versions
- **Internal links:** Use #gid=SheetID&range=A1 (different syntax)
- **File links:** Limited—primarily Google Drive links
- **Email links:** Full mailto: support
- **Security:** Opens links in new tab with warning
- **Display:** Shows as blue underlined text

### Key Differences

| Feature | Excel | Google Sheets |
|---------|-------|---------------|
| Internal link syntax | #Sheet!Cell | #gid=ID&range=Cell |
| Local file links | Full support | Not supported |
| Network path links | Full support | Not supported |
| Google Drive links | Via URL | Native support |
| SharePoint links | Full support | Via URL |
| Tel: protocol | Supported | Supported |

## Tips and Best Practices

1. **Always use friendly_name for readability:** `=HYPERLINK(url, "Click Here")` is much more user-friendly than showing raw URLs. Use descriptive text that indicates where the link goes.

2. **Use # for internal workbook navigation:** The hash symbol indicates an internal link. `#SheetName!A1` navigates within the workbook without needing the full file path.

3. **Use ENCODEURL for dynamic URL parameters:** When building URLs from cell values, `ENCODEURL(text)` properly escapes spaces and special characters: `"https://search.com?q="&ENCODEURL(A1)`.

4. **Validate links exist before displaying:** Wrap HYPERLINK in IF to hide links when source data is missing: `=IF(URL<>"", HYPERLINK(URL, "Link"), "")`.

5. **Single quotes for sheet names with spaces:** `#'My Sheet'!A1` requires quotes around sheet names containing spaces. Without quotes, the link fails silently.

6. **Consider security implications:** Hyperlinks from untrusted workbooks could lead to malicious sites. In enterprise settings, establish policies about workbook sources.

7. **Named ranges make stable internal links:** Instead of `#Sheet1!A1`, use `#DataStart` (a named range). Named ranges survive row/column insertions better than cell references.

8. **Test links after copying workbooks:** File path links become broken when workbooks move. Use relative paths or network paths that remain valid across locations.

## Related Functions

### Similar Functions

| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[ENCODEURL]] | Encodes text for use in URLs | Use within HYPERLINK to properly escape dynamic URL parameters |
| [[INDIRECT]] | Creates reference from text string | For internal navigation via cell references rather than clickable links |
| [[ADDRESS]] | Creates cell address as text | To construct the cell reference portion of internal HYPERLINK |

### Commonly Used Together

**[[ENCODEURL]]** - URL-encode text

*Combined with HYPERLINK for safe dynamic URLs:*
```
=HYPERLINK("https://google.com/search?q="&ENCODEURL(A1), "Search: "&A1)
```
ENCODEURL converts spaces and special characters to URL-safe format.

---

**[[CONCATENATE]] / [[&]]** - Join text strings

*Combined with HYPERLINK for URL construction:*
```
=HYPERLINK("https://site.com/"&Category&"/"&ProductID, ProductName)
```
Build complex URLs from multiple cell values.

---

**[[IF]]** - Conditional logic

*Combined with HYPERLINK for conditional links:*
```
=IF(A2<>"", HYPERLINK("https://example.com/"&A2, "View"), "")
```
Show links only when data exists; prevent broken links.

---

**[[VLOOKUP]] / [[INDEX]]** - Data lookup

*Combined with HYPERLINK for looked-up display text:*
```
=HYPERLINK(URLColumn, VLOOKUP(ID, ProductTable, NameColumn, FALSE))
```
Display text comes from a lookup while URL is in a data column.

---

**[[ROW]]** - Get row number

*Combined with HYPERLINK for row-specific links:*
```
=HYPERLINK("#Details!A"&ROW(), "Details")
```
Creates links to corresponding rows on another sheet as formula is copied down.

---

**[[TEXT]]** - Format numbers as text

*Combined with HYPERLINK for formatted display:*
```
=HYPERLINK("https://orders.com/"&OrderID, "Order #"&TEXT(OrderID,"000000"))
```
Formats the display text while using raw value for URL.

## Official Documentation

- **Microsoft Excel:** [HYPERLINK function](https://support.microsoft.com/en-us/office/hyperlink-function-333c7ce6-c5ae-4164-9c47-7de9b76f577f)
- **Google Sheets:** [HYPERLINK function](https://support.google.com/docs/answer/3093313)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel | Excel 97 (1997) | Original implementation |
| Excel 2007 | Enhanced | Better handling of long URLs |
| Excel 2016 | Enhanced | Improved security warnings |
| Excel 365 | Current | Full feature set with security controls |
| Google Sheets | Original launch (2006) | Different internal link syntax than Excel |

---

*Last updated: 2026-01-10*
