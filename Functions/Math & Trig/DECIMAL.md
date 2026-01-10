---
categories:
- spreadsheet-functions
subCategories:
- excel
- sheets
topics:
- math-trig
subTopics:
- number-conversion
- base-conversion
- number-systems
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- Convert to Decimal
- Base to Decimal
- Radix Conversion
tags:
- conversion
- binary
- hexadecimal
- octal
- number-systems
---

# DECIMAL

## Description

**DECIMAL** converts a text representation of a number in a specified base (radix) into a decimal (base 10) number. Give it "FF" and base 16, get 255. Give it "1010" and base 2, get 10. It's the reverse of the BASE function and the universal converter for any base from 2 to 36 back to decimal.

This function fills a gap left by specific functions like BIN2DEC, HEX2DEC, and OCT2DEC—it handles ANY base from 2 to 36. While those specialized functions only work for binary (2), octal (8), and hexadecimal (16), DECIMAL lets you convert from base 5, base 12, base 36, or any other radix. For bases above 10, letters A-Z represent digits 10-35.

**Text input, number output:** DECIMAL expects a text string as input (like "FF" or "1010") and returns an actual number that you can use in calculations. If you pass a number directly, it's converted to text first, which usually works but can cause confusion with leading zeros.

**Case insensitive:** DECIMAL accepts both uppercase and lowercase letters for digits above 9. "ff", "FF", and "Ff" all convert to 255 in base 16. This flexibility makes it easier to work with data from various sources.

## Syntax

> [!f(x)] DECIMAL Syntax
>
> ```
> =DECIMAL(text, radix)
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `text` | Yes | The text representation of the number to convert. Can contain digits 0-9 and letters A-Z. Must be valid for the specified radix. |
| `radix` | Yes | The base (radix) of the input number. Must be an integer between 2 and 36 inclusive. |

### Return Value

Returns the decimal (base 10) equivalent as a number. Returns #NUM! if text contains invalid characters for the specified radix, or if radix is outside 2-36. Returns #VALUE! if parameters are invalid types.

## Examples

> [!f(x)] DECIMAL Examples

### Example 1: Binary to Decimal
```
=DECIMAL("1010", 2)
```
**Result:** 10

**Explanation:** Binary 1010 = 1*8 + 0*4 + 1*2 + 0*1 = 10 in decimal.

---

### Example 2: Hexadecimal to Decimal
```
=DECIMAL("FF", 16)
```
**Result:** 255

**Explanation:** Hex FF = 15*16 + 15 = 255. The maximum value for one byte.

---

### Example 3: Octal to Decimal
```
=DECIMAL("100", 8)
```
**Result:** 64

**Explanation:** Octal 100 = 1*64 + 0*8 + 0*1 = 64. Useful for Unix permissions.

---

### Example 4: Lowercase Hex
```
=DECIMAL("ff", 16)
```
**Result:** 255

**Explanation:** Case doesn't matter. "ff" is the same as "FF" in hexadecimal.

---

### Example 5: Base 36 to Decimal
```
=DECIMAL("Z", 36)
```
**Result:** 35

**Explanation:** In base 36, Z represents 35 (the highest single digit). Useful for compact codes.

---

### Example 6: Larger Base 36 Number
```
=DECIMAL("LFLS", 36)
```
**Result:** 1000000

**Explanation:** Base 36 is very compact. "LFLS" equals one million in decimal.

---

### Example 7: Cell Reference
```
=DECIMAL(A1, B1)
```
Where A1 = "1100100", B1 = 2

**Result:** 100

**Explanation:** References allow dynamic conversion. Binary 1100100 = decimal 100.

---

### Example 8: Base 5 to Decimal
```
=DECIMAL("400", 5)
```
**Result:** 100

**Explanation:** Base 5: 4*25 + 0*5 + 0*1 = 100. DECIMAL handles any base 2-36.

---

### Example 9: Single Digit Conversion
```
=DECIMAL("A", 16)
```
**Result:** 10

**Explanation:** In hex, A = 10. Single characters work fine.

---

### Example 10: Leading Zeros
```
=DECIMAL("00FF", 16)
```
**Result:** 255

**Explanation:** Leading zeros are ignored. "00FF" and "FF" both equal 255.

---

### Example 11: Error Case - Invalid Character
```
=DECIMAL("G", 16)
```
**Result:** #NUM!

**Explanation:** G is not valid in base 16 (only 0-9 and A-F are valid). Returns error.

---

### Example 12: Error Case - Invalid Radix
```
=DECIMAL("10", 1)
```
**Result:** #NUM!

**Explanation:** Radix must be between 2 and 36. Base 1 is not supported.

---

### Example 13: Verification with BASE
```
=DECIMAL(BASE(255, 16), 16)
```
**Result:** 255

**Explanation:** Converting to hex and back returns the original. Useful for verification.

---

### Example 14: Color Code Conversion
```
=DECIMAL(MID("#FF5733", 2, 2), 16)
```
**Result:** 255

**Explanation:** Extract and convert the red component from a hex color code. #FF5733 has R=255.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#NUM!` | Text contains characters invalid for the specified radix | Verify all characters are valid. Base 2 only allows 0-1. Base 16 allows 0-9 and A-F. |
| `#NUM!` | Radix is less than 2 or greater than 36 | Ensure radix is an integer between 2 and 36 inclusive. |
| `#VALUE!` | Non-text first parameter or non-numeric radix | Ensure text is a string and radix is a number. |
| `Unexpected result` | Passing a number instead of text | Use TEXT() to explicitly convert, or ensure the cell is formatted as text. Numbers lose leading zeros. |
| `Wrong result` | Confusing number formats | "10" in binary is 2, in decimal is 10, in hex is 16. Make sure radix matches your input format. |

## Use Cases

### [[Color Code Processing]]

**Scenario:** Convert hex color codes to RGB decimal values for calculations or display.

**Implementation:**
```
Red: =DECIMAL(MID(A1, 2, 2), 16)
Green: =DECIMAL(MID(A1, 4, 2), 16)
Blue: =DECIMAL(MID(A1, 6, 2), 16)
```
Where A1 contains "#FF5733"

**Business Application:** Web development, graphic design, data visualization. Convert hex colors to RGB for manipulation or display.

**Technical Details:** Extract 2-character hex pairs from color strings, convert each to 0-255 decimal values for RGB representation.

---

### [[Binary Data Analysis]]

**Scenario:** Convert binary strings from sensor data, network packets, or configuration files to decimal values.

**Implementation:**
```
Value: =DECIMAL(binary_string, 2)
```

**Business Application:** IoT data processing, network analysis, embedded systems debugging. Binary data often needs decimal conversion for analysis.

**Technical Details:** Works with binary strings of any length (within precision limits). Useful for interpreting bit fields and flags.

---

### [[Legacy System Integration]]

**Scenario:** Convert data from systems using non-decimal number formats (octal permissions, hex memory addresses).

**Implementation:**
```
Unix Permission: =DECIMAL(permission_octal, 8)
Memory Address: =DECIMAL(hex_address, 16)
```

**Business Application:** System administration, database migration, mainframe integration. Many legacy systems use non-decimal formats.

**Technical Details:** Unix permissions (755, 644) are octal. Memory addresses and MAC addresses are hexadecimal.

---

### [[Short Code Decoding]]

**Scenario:** Decode compact base-36 codes back to sequential IDs.

**Implementation:**
```
Original ID: =DECIMAL(short_code, 36) - 100000
```

**Business Application:** URL shorteners, order tracking, coupon systems. Decode user-facing codes to internal IDs.

**Technical Details:** Reverse of BASE function for code generation. Subtract any offset applied during encoding.

## Platform Differences

### Microsoft Excel
- **Availability:** Excel 2013 and later
- **Maximum value:** Limited by Excel's numeric precision (about 10^15)
- **Case handling:** Case-insensitive (A-Z and a-z both valid for digits 10-35)
- **Leading zeros:** Accepted and ignored

### Google Sheets
- **Availability:** All versions
- **Maximum value:** Same practical limits as Excel
- **Case handling:** Case-insensitive (same as Excel)
- **Leading zeros:** Accepted and ignored

### Both Platforms
- Identical function name and syntax
- Same radix range (2-36)
- Same error conditions
- Returns a number (not text)

### Comparison with Specific Functions
```
BIN2DEC("1010") = 10         DECIMAL("1010", 2) = 10
HEX2DEC("FF") = 255          DECIMAL("FF", 16) = 255
OCT2DEC("100") = 64          DECIMAL("100", 8) = 64
```
DECIMAL is more flexible; specific functions may have different limits.

## Tips and Best Practices

1. **Use text input:** Always pass the first argument as text (in quotes or from a text-formatted cell). Numbers may lose leading zeros or be misinterpreted.

2. **Case doesn't matter:** "FF", "ff", and "Ff" are all equivalent for hexadecimal. No need to standardize case before conversion.

3. **Verify with roundtrip:** Use `=BASE(DECIMAL(x, r), r)` to verify conversions. Should return the original (without leading zeros).

4. **Know valid characters:** Each radix has specific valid characters. Base 2: 0-1. Base 8: 0-7. Base 16: 0-9, A-F. Base 36: 0-9, A-Z.

5. **Handle leading zeros:** DECIMAL ignores leading zeros. "007" in any base is the same as "7".

6. **Combine with text functions:** Use MID, LEFT, RIGHT to extract portions of strings before conversion (e.g., extracting color components).

7. **Remember precision limits:** For very large results, Excel's 15-digit precision may cause issues. Test with your expected value range.

## Related Functions

### Similar Functions
| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[BASE]] | Converts decimal to any base | For the reverse conversion |
| [[BIN2DEC]] | Converts binary to decimal | When you specifically have binary with its limits |
| [[HEX2DEC]] | Converts hexadecimal to decimal | When you specifically have hex with its limits |
| [[OCT2DEC]] | Converts octal to decimal | When you specifically have octal with its limits |

### Commonly Used Together

**[[BASE]]** - Reverse conversion

*Roundtrip verification:*
```
=BASE(DECIMAL("FF", 16), 16)  // Returns "FF"
```
Convert to decimal and back.

---

**[[MID]] / [[LEFT]] / [[RIGHT]]** - String extraction

*Extract portions before converting:*
```
=DECIMAL(MID(A1, 2, 2), 16)  // Convert middle 2 chars as hex
```
Essential for parsing formatted strings like color codes.

---

**[[TEXT]]** - Ensure text input

*Convert numbers to text:*
```
=DECIMAL(TEXT(A1, "0"), 2)  // Force text interpretation
```
Prevents issues when source cells contain numbers.

---

**[[CONCATENATE]] / [[&]]** - Build input strings

*Combine components:*
```
=DECIMAL(A1 & B1, 16)  // Combine two hex values
```
Build the text string to convert.

---

**[[IFERROR]]** - Handle invalid input

*Graceful error handling:*
```
=IFERROR(DECIMAL(A1, 16), "Invalid hex")
```
Catch errors from invalid characters.

## Official Documentation

- **Microsoft Excel:** [DECIMAL function](https://support.microsoft.com/en-us/office/decimal-function-ee554665-6176-46ef-82de-0a283658da2e)
- **Google Sheets:** [DECIMAL function](https://support.google.com/docs/answer/9061514) (Note: Sheets documentation may list this under different category)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Excel | Excel 2013 | Initial implementation |
| Excel 2016+ | Enhanced | Performance improvements |
| Google Sheets | 2019 | Added to match Excel functionality |

---

*Last updated: 2026-01-10*
