---
categories:
  - spreadsheet-functions
subCategories:
  - excel
  - sheets
topics:
  - text-manipulation
  - number-formatting
subTopics:
  - thai-baht
  - currency-text
  - number-to-words
  - thai-language
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
  - thai-baht-text
  - baht-to-words
  - thai-currency-text
  - number-to-thai
tags:
  - text
  - currency
  - thai
  - localization
  - number-conversion
  - excel
  - google-sheets
---

# BAHTTEXT

## Description

BAHTTEXT is a specialized text function that converts a number to Thai text representation of the amount in Thai Baht currency, including the Satang (subunit, 1/100 of a Baht). The function outputs the numeric value spelled out in Thai script, following Thai linguistic conventions for expressing monetary amounts. For example, the number 1234.50 would be converted to Thai text representing "one thousand two hundred thirty-four Baht fifty Satang." This is the standard format used in Thai financial documents, checks, invoices, and legal documents where amounts must be expressed in words.

BAHTTEXT is primarily used in Thai business and financial contexts where regulations or conventions require amounts to be written in words rather than (or in addition to) digits. Common applications include generating check amounts, creating official invoices and receipts, preparing legal financial documents, producing Thai tax documents, and formatting financial reports for Thai audiences. The function handles both the integer Baht portion and the Satang decimal portion, outputting the complete Thai text representation as expected in official documents.

There are several important gotchas to understand with BAHTTEXT. First, the function only works with numeric values; text that looks like numbers will cause errors. Second, the output is purely in Thai script; there is no option for transliteration to Latin characters. Third, the function rounds to two decimal places (Satang), so any precision beyond that is lost. Fourth, negative numbers produce output but include the Thai word for "minus," which may not be appropriate in all contexts. Fifth, very large numbers are supported but may produce very long text strings. Sixth, zero produces the Thai text for "zero Baht," not an empty string.

Regarding platform availability, BAHTTEXT is available in Microsoft Excel across all versions with Thai language support. In Google Sheets, BAHTTEXT is also available with identical functionality. Both platforms produce the same Thai text output for the same input values. The function is most useful in Thai localized environments but works in any version of Excel or Sheets where it is available. For non-Thai currency text conversion, Excel offers NUMBERSTRING (for Chinese/Japanese) but has no built-in equivalent for other languages; custom solutions are typically required.

## Syntax

> [!f(x)] BAHTTEXT Syntax
>
> ```excel
> =BAHTTEXT(number)
> ```
>
> **Parameters:**
>
> | Parameter | Description | Required |
> |-----------|-------------|----------|
> | `number` | The numeric value to convert to Thai Baht text. Can be a number, cell reference, or formula that returns a number. Values are rounded to two decimal places. | Yes |

## Examples

### Example 1: Basic Whole Number
```excel
=BAHTTEXT(1)
```
**Result:** `หนึ่งบาทถ้วน`

**Explanation:** The number 1 is converted to Thai text meaning "one Baht exactly." The word "ถ้วน" (tuan) indicates an exact amount with no Satang.

---

### Example 2: Number with Satang
```excel
=BAHTTEXT(1.50)
```
**Result:** `หนึ่งบาทห้าสิบสตางค์`

**Explanation:** 1.50 Baht is expressed as "one Baht fifty Satang" in Thai. The decimal portion is converted to the Satang subunit.

---

### Example 3: Larger Amount
```excel
=BAHTTEXT(1234)
```
**Result:** `หนึ่งพันสองร้อยสามสิบสี่บาทถ้วน`

**Explanation:** 1,234 Baht spelled out in Thai: "one thousand two hundred thirty-four Baht exactly."

---

### Example 4: Typical Invoice Amount
```excel
=BAHTTEXT(42500.75)
```
**Result:** `สี่หมื่นสองพันห้าร้อยบาทเจ็ดสิบห้าสตางค์`

**Explanation:** Common invoice amount showing both Baht and Satang portions fully spelled out in Thai.

---

### Example 5: Zero Value
```excel
=BAHTTEXT(0)
```
**Result:** `ศูนย์บาทถ้วน`

**Explanation:** Zero is converted to Thai text for "zero Baht exactly." It does not return an empty string.

---

### Example 6: Cell Reference
```excel
=BAHTTEXT(A1)
```
Where A1 contains 999.99

**Result:** `เก้าร้อยเก้าสิบเก้าบาทเก้าสิบเก้าสตางค์`

**Explanation:** BAHTTEXT works with cell references. The value 999.99 becomes "nine hundred ninety-nine Baht ninety-nine Satang."

---

### Example 7: Formula Result
```excel
=BAHTTEXT(SUM(A1:A10))
```
Where SUM equals 5000

**Result:** `ห้าพันบาทถ้วน`

**Explanation:** BAHTTEXT can take formula results as input. The sum of 5,000 becomes "five thousand Baht exactly."

---

### Example 8: Negative Number
```excel
=BAHTTEXT(-100)
```
**Result:** `ลบหนึ่งร้อยบาทถ้วน`

**Explanation:** Negative numbers include the Thai word for "minus" (ลบ) at the beginning. Use with caution in financial documents.

---

### Example 9: Large Amount (Million)
```excel
=BAHTTEXT(1000000)
```
**Result:** `หนึ่งล้านบาทถ้วน`

**Explanation:** One million Baht is expressed as "one million Baht exactly." Thai uses "ล้าน" (lan) for million.

---

### Example 10: Satang Only (Less than 1 Baht)
```excel
=BAHTTEXT(0.25)
```
**Result:** `ยี่สิบห้าสตางค์`

**Explanation:** Amounts less than 1 Baht show only the Satang portion: "twenty-five Satang."

---

### Example 11: Rounding Behavior
```excel
=BAHTTEXT(100.999)
```
**Result:** `หนึ่งร้อยเอ็ดบาทถ้วน`

**Explanation:** Values are rounded to 2 decimal places. 100.999 rounds to 101.00, becoming "one hundred one Baht exactly."

---

### Example 12: Common Check Amount
```excel
=BAHTTEXT(25000)
```
**Result:** `สองหมื่นห้าพันบาทถ้วน`

**Explanation:** A typical check amount showing "twenty-five thousand Baht exactly."

---

### Example 13: With ROUND Function
```excel
=BAHTTEXT(ROUND(A1, 2))
```
Where A1 contains 1234.5678

**Result:** `หนึ่งพันสองร้อยสามสิบสี่บาทห้าสิบเจ็ดสตางค์`

**Explanation:** While BAHTTEXT rounds automatically, using ROUND explicitly documents the intent and ensures consistent behavior.

---

### Example 14: Very Large Amount
```excel
=BAHTTEXT(999999999.99)
```
**Result:** `เก้าร้อยเก้าสิบเก้าล้านเก้าแสนเก้าหมื่นเก้าพันเก้าร้อยเก้าสิบเก้าบาทเก้าสิบเก้าสตางค์`

**Explanation:** Very large amounts produce long text strings. This represents nearly 1 billion Baht with 99 Satang.

---

### Example 15: Combined with Concatenation
```excel
="Amount: " & BAHTTEXT(A1)
```
Where A1 contains 500

**Result:** `Amount: ห้าร้อยบาทถ้วน`

**Explanation:** BAHTTEXT can be concatenated with other text for labels and document formatting.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#VALUE!` | Non-numeric input (text that looks like a number) | Ensure input is a true number; use VALUE() to convert text if needed |
| `#VALUE!` | Empty cell reference | Check that referenced cells contain values |
| `#NAME?` | Function not recognized (rare) | Verify Excel/Sheets version supports BAHTTEXT |
| `Unexpected output` | Negative numbers | Be aware that negative values include "minus" prefix |
| `Very long string` | Large numbers | Expected behavior; consider display width in documents |
| `Rounding differences` | More than 2 decimal places | BAHTTEXT rounds to 2 decimals; round explicitly if needed |

## Use Cases

### [[Thai Check Printing]]
- **Implementation**: Use BAHTTEXT to convert payment amounts to Thai text for the "Amount in words" field on checks. Combine with date formatting and payee information for complete check generation.
- **Business Application**: Banks and businesses in Thailand require checks to have amounts written in both digits and Thai words. Automated check printing systems use BAHTTEXT to generate the text portion, reducing errors and improving efficiency.
- **Technical Details**: Format the BAHTTEXT output cell with appropriate Thai font and size. Consider adding "***" or other security markers around the text. Validate that amounts match between numeric and text fields before printing.

### [[Thai Invoice Generation]]
- **Implementation**: Generate official Thai invoices with amounts expressed in Thai text. Include both the numeric total and BAHTTEXT representation as required by Thai accounting standards.
- **Business Application**: Thai businesses, especially those dealing with government entities or formal B2B transactions, need invoices with amounts in Thai words. This is often a legal or regulatory requirement for official documents.
- **Technical Details**: Place BAHTTEXT output in a designated "amount in words" field. Ensure proper Thai font rendering. Consider document templates that include both numeric tables and text amount fields.

### [[Thai Receipt Printing]]
- **Implementation**: Add Thai text amount representation to printed receipts for formal transactions. Particularly important for high-value sales or tax-related transactions.
- **Business Application**: Retail POS systems serving Thai customers, especially for significant purchases, benefit from receipts showing amounts in Thai words for clarity and formality.
- **Technical Details**: Optimize text length for receipt printer width. Consider truncation or abbreviation for very large amounts. Test Thai character rendering on target receipt printers.

### [[Thai Financial Reports]]
- **Implementation**: Include Thai text representation of key financial figures in reports destined for Thai audiences, board presentations, or regulatory submissions.
- **Business Application**: Annual reports, financial statements, and regulatory filings for Thai entities may require or benefit from amounts expressed in Thai words, particularly for summary totals.
- **Technical Details**: Use BAHTTEXT for summary totals and key figures. Consider readability of long number strings. Ensure proper formatting in exported PDFs or printed documents.

## Platform Differences

| Feature | Excel | Google Sheets |
|---------|-------|---------------|
| **Availability** | All versions with Thai support | All versions |
| **Function Syntax** | =BAHTTEXT(number) | =BAHTTEXT(number) |
| **Output** | Thai script text | Thai script text |
| **Negative Numbers** | Includes "ลบ" (minus) | Includes "ลบ" (minus) |
| **Zero Handling** | Returns "ศูนย์บาทถ้วน" | Returns "ศูนย์บาทถ้วน" |
| **Rounding** | 2 decimal places | 2 decimal places |
| **Maximum Value** | Very large numbers supported | Very large numbers supported |
| **Font Requirements** | Thai font for display | Thai font for display |

**Key Notes:**

- BAHTTEXT is fully compatible between Excel and Google Sheets
- Both platforms produce identical Thai text output
- Proper Thai font must be available for correct display
- The function is specialized for Thai Baht only; no equivalent exists for other currencies

**Alternative Approaches for Other Currencies:**

For number-to-text conversion in other languages:
- Excel has NUMBERSTRING for Chinese/Japanese
- Custom VBA or Apps Script functions can be created
- Third-party add-ins may provide additional language support

## Tips and Best Practices

1. **Validate Input Types**: Ensure the input is a true number, not text. Use VALUE() if converting from text, or check cell formatting.

2. **Handle Negative Values Carefully**: Negative numbers produce text with "minus," which may not be appropriate for financial documents. Use ABS() if negatives should not occur.

3. **Consider Display Font**: Thai text requires proper Thai font support. Ensure your document, export format, or print system supports Thai script rendering.

4. **Round Explicitly When Needed**: While BAHTTEXT rounds to 2 decimals automatically, using ROUND() explicitly documents the intent and ensures consistent behavior.

5. **Test Large Numbers**: Very large amounts produce long text strings. Test with realistic maximum values to ensure proper display in your layout.

6. **Combine with Numeric Display**: Best practice in Thai documents is to show both numeric format (1,234.50) and text format (หนึ่งพัน...) for verification.

7. **Use for Official Documents Only**: BAHTTEXT is primarily for formal, official documents. Informal contexts typically use numeric representation only.

8. **Verify Check Amount Formats**: When printing checks, verify that your bank's check format accommodates the text length generated by BAHTTEXT.

## Related Functions

### Similar Functions

| Function | Description | Key Difference from BAHTTEXT |
|----------|-------------|------------------------------|
| [[NUMBERSTRING]] | Converts numbers to Chinese/Japanese text | Different language output; Chinese/Japanese number words |
| [[TEXT]] | Formats numbers as text with format codes | General formatting, not word representation |
| [[FIXED]] | Formats number with fixed decimals as text | Numeric text representation, not word spelling |
| [[DOLLAR]] | Formats as currency text | Currency symbol formatting, not words |

### Commonly Used Together

**[[ROUND]]** - Round before conversion

*Ensure specific rounding:*
```excel
=BAHTTEXT(ROUND(A1, 2))
```
Explicitly round to 2 decimal places before conversion.

**[[ABS]]** - Handle negative values

*Convert to positive for text:*
```excel
=BAHTTEXT(ABS(A1))
```
Remove negative sign when not appropriate for document context.

**[[SUM]]** - Calculate totals

*Convert sum to text:*
```excel
=BAHTTEXT(SUM(B2:B100))
```
Convert invoice line item totals to Thai text.

**[[VALUE]]** - Convert text to number

*Handle text-formatted numbers:*
```excel
=BAHTTEXT(VALUE(A1))
```
Convert text that looks like a number before using BAHTTEXT.

**[[CONCATENATE]]/[[&]]** - Build document text

*Add labels or context:*
```excel
="Total Amount: " & BAHTTEXT(A1)
```
Combine BAHTTEXT output with descriptive labels.

**[[IF]]** - Conditional output

*Handle special cases:*
```excel
=IF(A1>0, BAHTTEXT(A1), "No amount")
```
Provide alternative output for zero or negative values.

## Official Documentation

- **Microsoft Excel**: [BAHTTEXT function](https://support.microsoft.com/en-us/office/bahttext-function-5ba4d0b4-abd3-4325-8d22-7a92d59aab9c)
- **Google Sheets**: [BAHTTEXT function](https://support.google.com/docs/answer/3093320)

## Version History

| Version | Date | Changes |
|---------|------|---------|
| Excel 97 | 1997 | Original implementation for Thai localization |
| Excel 2000 | 2000 | Enhanced Thai text generation |
| Excel 2007 | 2007 | Unicode improvements |
| Google Sheets | Original | Available since launch |
| Excel 365 | Ongoing | Continued support in modern versions |

---

*Last updated: 2026-01-10*
