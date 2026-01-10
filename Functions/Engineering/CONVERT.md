---
categories:
- spreadsheet-functions
subCategories:
- excel
- sheets
topics:
- engineering
subTopics:
- unit-conversion
- measurement-systems
- dimensional-analysis
- scientific-calculations
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
- unit-conversion
- measurement-conversion
- convert-units
tags:
- engineering-function
- unit-conversion
- metric-imperial
- dimensional-analysis
- scientific-units
---

# CONVERT

## Description

CONVERT transforms a numeric value from one unit of measurement to another, supporting over 100 different units across 13 measurement categories including weight, distance, time, temperature, volume, area, energy, power, force, magnetism, pressure, speed, and information. This function is indispensable for engineers, scientists, and analysts who work with data from multiple measurement systems or need to standardize values for calculations, reporting, or international collaboration.

The function performs dimensional analysis by applying the appropriate conversion factor between the source and target units. For most conversions, this involves a simple multiplication or division by a constant factor (e.g., 1 inch = 2.54 centimeters). Temperature conversions are more complex, requiring both multiplication and addition operations to handle the different zero points of Celsius, Fahrenheit, and Kelvin scales. CONVERT handles these differences automatically, ensuring mathematically correct transformations regardless of the unit types involved.

Unit abbreviations in CONVERT follow international standards where possible, using SI (International System of Units) prefixes for metric units. The function recognizes binary prefixes (kibi, mebi, gibi) for information units, distinguishing between decimal-based prefixes (kilo = 1000) and binary-based prefixes (kibi = 1024). Both metric and imperial/US customary units are supported, making the function equally useful for users in countries using different measurement standards. Unit abbreviations are case-sensitive, so "m" (meters) is different from "M" (statute mile in some contexts).

CONVERT is available in both Excel and Google Sheets with nearly identical functionality. Excel supports a broader range of units including some legacy and regional measurements, while Google Sheets covers all common scientific and engineering units. Both platforms support metric prefix combinations (adding "k" for kilo, "m" for milli, etc.) to base units, exponentially expanding the number of possible conversions. The function returns #N/A if the units are incompatible (different measurement types) and #VALUE! if the unit abbreviation is not recognized.

## Syntax

> [!NOTE] Syntax
> ```
> CONVERT(number, from_unit, to_unit)
> ```

### Parameters

| Parameter | Description | Required |
|-----------|-------------|----------|
| **number** | The numeric value to convert. Can be any real number, positive, negative, or zero. | Yes |
| **from_unit** | A text string representing the source unit of measurement. Must be enclosed in quotation marks. Case-sensitive. | Yes |
| **to_unit** | A text string representing the target unit of measurement. Must be enclosed in quotation marks. Case-sensitive. | Yes |

## Unit Reference Tables

### Weight and Mass Units

| Unit | Abbreviation | Description |
|------|--------------|-------------|
| Gram | "g" | SI base unit for mass (1/1000 kg) |
| Slug | "sg" | Imperial unit of mass |
| Pound mass | "lbm" | Avoirdupois pound |
| U (atomic mass unit) | "u" | Dalton, 1/12 mass of carbon-12 |
| Ounce mass | "ozm" | Avoirdupois ounce (1/16 pound) |
| Grain | "grain" | 1/7000 of a pound |
| Stone | "stone" | 14 pounds (UK) |
| Ton | "ton" | Short ton (2000 pounds) |
| Imperial ton | "uk_ton" or "LTON" | Long ton (2240 pounds) |
| Metric ton | "tonne" | 1000 kilograms |

### Distance/Length Units

| Unit | Abbreviation | Description |
|------|--------------|-------------|
| Meter | "m" | SI base unit for length |
| Statute mile | "mi" | 5280 feet |
| Nautical mile | "Nmi" | 1852 meters |
| Inch | "in" | 1/12 foot |
| Foot | "ft" | 12 inches |
| Yard | "yd" | 3 feet |
| Angstrom | "ang" | 10^-10 meters |
| Ell | "ell" | 45 inches |
| Light-year | "ly" | Distance light travels in one year |
| Parsec | "parsec" or "pc" | 3.26 light-years |
| Pica (1/72 inch) | "Picapt" or "Pica" | Typography unit |
| Pica (1/6 inch) | "pica" | Typography unit (Excel) |
| Survey mile | "survey_mi" | US survey mile |

### Time Units

| Unit | Abbreviation | Description |
|------|--------------|-------------|
| Year | "yr" | 365.25 days |
| Day | "day" or "d" | 24 hours |
| Hour | "hr" | 60 minutes |
| Minute | "mn" or "min" | 60 seconds |
| Second | "sec" or "s" | SI base unit for time |

### Temperature Units

| Unit | Abbreviation | Description |
|------|--------------|-------------|
| Celsius | "C" or "cel" | Centigrade scale |
| Fahrenheit | "F" or "fah" | Imperial temperature scale |
| Kelvin | "K" or "kel" | SI absolute temperature |
| Rankine | "Rank" | Absolute scale based on Fahrenheit |
| Reaumur | "Reau" | Historical temperature scale |

### Volume/Liquid Measure Units

| Unit | Abbreviation | Description |
|------|--------------|-------------|
| Teaspoon | "tsp" | US teaspoon |
| Modern teaspoon | "tspm" | Metric teaspoon (5 mL) |
| Tablespoon | "tbs" | US tablespoon (3 teaspoons) |
| Fluid ounce | "oz" | US fluid ounce |
| Cup | "cup" | US cup (8 fluid ounces) |
| US pint | "pt" or "us_pt" | 16 US fluid ounces |
| UK pint | "uk_pt" | Imperial pint |
| Quart | "qt" | US quart (2 pints) |
| Imperial quart | "uk_qt" | Imperial quart |
| Gallon | "gal" | US gallon (4 quarts) |
| Imperial gallon | "uk_gal" | Imperial gallon |
| Liter | "l" or "L" or "lt" | 1000 cubic centimeters |
| Cubic angstrom | "ang3" or "ang^3" | Angstrom cubed |
| Barrel | "barrel" | US oil barrel (42 gallons) |
| Bushel | "bushel" | US bushel |
| Cubic feet | "ft3" or "ft^3" | Foot cubed |
| Cubic inch | "in3" or "in^3" | Inch cubed |
| Cubic light-year | "ly3" or "ly^3" | Light-year cubed |
| Cubic meter | "m3" or "m^3" | Meter cubed |
| Cubic mile | "mi3" or "mi^3" | Mile cubed |
| Cubic yard | "yd3" or "yd^3" | Yard cubed |
| Cubic nautical mile | "Nmi3" or "Nmi^3" | Nautical mile cubed |
| Cubic Pica | "Picapt3" or "Picapt^3" | Pica cubed |
| Gross registered ton | "GRT" or "regton" | 100 cubic feet |
| Measurement ton | "MTON" | 40 cubic feet |

### Area Units

| Unit | Abbreviation | Description |
|------|--------------|-------------|
| Square meter | "m2" or "m^2" | SI area unit |
| Square feet | "ft2" or "ft^2" | Foot squared |
| Square inch | "in2" or "in^2" | Inch squared |
| Square yard | "yd2" or "yd^2" | Yard squared |
| Square mile | "mi2" or "mi^2" | Mile squared |
| Hectare | "ha" | 10,000 square meters |
| Acre | "uk_acre" or "acre" | 43,560 square feet |
| International acre | "us_acre" | US survey acre |
| Morgen | "Morgen" | South African/German unit |
| Are | "ar" | 100 square meters |

### Energy Units

| Unit | Abbreviation | Description |
|------|--------------|-------------|
| Joule | "J" | SI unit of energy |
| Erg | "e" | CGS unit (10^-7 joules) |
| Thermodynamic calorie | "c" | 4.184 joules |
| IT calorie | "cal" | International Table calorie |
| Electron volt | "eV" or "ev" | Energy of one electron through 1 volt |
| Horsepower-hour | "HPh" or "hh" | 2,684,519.5 joules |
| Watt-hour | "Wh" or "wh" | 3600 joules |
| Foot-pound | "flb" | 1.355818 joules |
| BTU | "BTU" or "btu" | British Thermal Unit |

### Power Units

| Unit | Abbreviation | Description |
|------|--------------|-------------|
| Watt | "W" or "w" | SI unit of power (J/s) |
| Horsepower | "HP" or "h" | 745.7 watts |
| Pferdestärke | "PS" | Metric horsepower (735.5 watts) |

### Force Units

| Unit | Abbreviation | Description |
|------|--------------|-------------|
| Newton | "N" | SI unit of force |
| Dyne | "dyn" or "dy" | CGS unit (10^-5 newtons) |
| Pound force | "lbf" | Force of one pound mass under standard gravity |
| Pond | "pond" | Force of one gram under standard gravity |

### Pressure Units

| Unit | Abbreviation | Description |
|------|--------------|-------------|
| Pascal | "Pa" or "p" | SI unit (N/m^2) |
| Atmosphere | "atm" or "at" | Standard atmosphere |
| mmHg | "mmHg" | Millimeters of mercury |
| PSI | "psi" | Pounds per square inch |
| Torr | "Torr" | 1/760 of an atmosphere |

### Magnetism Units

| Unit | Abbreviation | Description |
|------|--------------|-------------|
| Tesla | "T" | SI unit of magnetic flux density |
| Gauss | "ga" | CGS unit (10^-4 tesla) |

### Speed Units

| Unit | Abbreviation | Description |
|------|--------------|-------------|
| Meters per second | "m/s" or "m/sec" | SI speed unit |
| Meters per hour | "m/hr" or "m/h" | Meters per hour |
| Miles per hour | "mph" | Statute miles per hour |
| Knot | "kn" or "knot" | Nautical miles per hour |
| Admiralty knot | "admkn" | UK nautical miles per hour |

### Information Units

| Unit | Abbreviation | Description |
|------|--------------|-------------|
| Bit | "bit" | Binary digit |
| Byte | "byte" | 8 bits |

### Metric Prefixes (Can be combined with base units)

| Prefix | Symbol | Multiplier |
|--------|--------|------------|
| Yotta | "Y" | 10^24 |
| Zetta | "Z" | 10^21 |
| Exa | "E" | 10^18 |
| Peta | "P" | 10^15 |
| Tera | "T" | 10^12 |
| Giga | "G" | 10^9 |
| Mega | "M" | 10^6 |
| Kilo | "k" | 10^3 |
| Hecto | "h" | 10^2 |
| Deka | "da" or "e" | 10^1 |
| Deci | "d" | 10^-1 |
| Centi | "c" | 10^-2 |
| Milli | "m" | 10^-3 |
| Micro | "u" | 10^-6 |
| Nano | "n" | 10^-9 |
| Pico | "p" | 10^-12 |
| Femto | "f" | 10^-15 |
| Atto | "a" | 10^-18 |
| Zepto | "z" | 10^-21 |
| Yocto | "y" | 10^-24 |

### Binary Prefixes (For information units)

| Prefix | Symbol | Multiplier |
|--------|--------|------------|
| Kibi | "ki" | 2^10 = 1,024 |
| Mebi | "Mi" | 2^20 = 1,048,576 |
| Gibi | "Gi" | 2^30 = 1,073,741,824 |
| Tebi | "Ti" | 2^40 |
| Pebi | "Pi" | 2^50 |
| Exbi | "Ei" | 2^60 |
| Zebi | "Zi" | 2^70 |
| Yobi | "Yi" | 2^80 |

## Examples

| Formula | Result | Explanation |
|---------|--------|-------------|
| `=CONVERT(1, "mi", "km")` | 1.609344 | Converts 1 mile to kilometers |
| `=CONVERT(100, "C", "F")` | 212 | Converts 100 degrees Celsius to Fahrenheit |
| `=CONVERT(32, "F", "C")` | 0 | Converts 32 degrees Fahrenheit to Celsius |
| `=CONVERT(1, "lbm", "kg")` | 0.453592 | Converts 1 pound to kilograms |
| `=CONVERT(1, "gal", "L")` | 3.785412 | Converts 1 US gallon to liters |
| `=CONVERT(1, "in", "cm")` | 2.54 | Converts 1 inch to centimeters (exact) |
| `=CONVERT(1024, "byte", "kibibyte")` | 1 | Converts 1024 bytes to 1 kibibyte |
| `=CONVERT(1, "J", "cal")` | 0.239006 | Converts 1 joule to calories |
| `=CONVERT(100, "km/h", "mph")` | 62.1371 | Converts 100 km/h to miles per hour (via m/s) |
| `=CONVERT(1, "atm", "Pa")` | 101325 | Converts 1 atmosphere to Pascals |

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#N/A` | Incompatible unit types (e.g., mass to length) | Ensure from_unit and to_unit measure the same physical quantity |
| `#VALUE!` | Unrecognized unit abbreviation | Check spelling and case-sensitivity of unit strings |
| `#VALUE!` | Non-text unit argument | Enclose unit abbreviations in quotation marks |
| `#VALUE!` | Non-numeric number argument | Ensure the first argument is a numeric value |
| `#N/A` | Unsupported prefix-unit combination | Not all units accept all prefixes; check documentation |
| `#VALUE!` | Empty string for unit | Provide valid unit abbreviations, not empty strings |
| `#VALUE!` | Wrong case (e.g., "M" vs "m") | Unit abbreviations are case-sensitive; "m" = meters, "M" = mega prefix |

## Use Cases

### 1. International Engineering Documentation

**Scenario**: A mechanical engineer needs to convert a technical specification document from imperial units to metric units for international clients. Dimensions, weights, and pressures must all be converted systematically.

**Implementation**: Create a conversion worksheet that transforms all imperial measurements to their metric equivalents, ensuring consistency and accuracy across the entire document.

```
| Component | Imperial Value | Unit | Formula | Metric Value | Metric Unit |
|-----------|----------------|------|---------|--------------|-------------|
| Length    | 24             | in   | =CONVERT(B2,"in","mm") | 609.6 | mm |
| Weight    | 15             | lbm  | =CONVERT(B3,"lbm","kg") | 6.804 | kg |
| Pressure  | 100            | psi  | =CONVERT(B4,"psi","Pa") | 689476 | Pa |
| Torque    | 50             | flb  | =CONVERT(B5,"flb","J") | 67.79 | N-m |
| Volume    | 2.5            | gal  | =CONVERT(B6,"gal","L") | 9.464 | L |
```

### 2. Scientific Data Normalization

**Scenario**: A research scientist has collected data from multiple sources using different unit systems. All measurements must be normalized to SI units before analysis and publication.

**Implementation**: Build a data processing pipeline that automatically converts all input measurements to standard SI units regardless of their original format.

```
| Sample | Raw Value | Original Unit | Conversion Formula | SI Value | SI Unit |
|--------|-----------|---------------|-------------------|----------|---------|
| A-001  | 25        | C             | =CONVERT(B2,"C","K") | 298.15 | K |
| A-002  | 1.5       | cal           | =CONVERT(B3,"cal","J") | 6.276 | J |
| A-003  | 760       | mmHg          | =CONVERT(B4,"mmHg","Pa") | 101325 | Pa |
| A-004  | 100       | cm            | =CONVERT(B5,"cm","m") | 1 | m |
```

### 3. Data Storage Capacity Planning

**Scenario**: An IT administrator needs to convert between different data storage units to plan server capacity, comparing specifications from different vendors who use inconsistent unit conventions.

**Implementation**: Create a standardized comparison sheet that converts all storage capacities to a common unit (e.g., gibibytes) to enable accurate vendor comparisons.

```
| Server | Advertised | Unit | Formula | Actual GiB |
|--------|------------|------|---------|------------|
| Vendor A | 1000 | GB | =CONVERT(B2*1e9,"byte","Gibyte") | 931.32 |
| Vendor B | 1 | TB | =CONVERT(B3*1e12,"byte","Gibyte") | 931.32 |
| Vendor C | 1024 | GiB | =B4 | 1024 |
| Total Required | - | - | =SUM(E2:E4) | 2886.64 |

Note: Marketing "GB" often means 10^9 bytes, not 2^30 bytes
Binary GiB (gibibytes) = 2^30 bytes for accurate comparison
```

## Platform Differences

| Feature | Excel | Google Sheets |
|---------|-------|---------------|
| Total supported units | ~100+ | ~80+ |
| Metric prefixes | Full support | Full support |
| Binary prefixes | kibi, mebi, gibi, etc. | kibi, mebi, gibi, etc. |
| Temperature scales | C, F, K, Rank, Reau | C, F, K, Rank |
| Speed units | m/s, m/h, mph, kn | m/s, m/h, mph, kn |
| Case sensitivity | Yes | Yes |
| Legacy unit aliases | More extensive | Limited |
| Survey mile | Supported | Not supported |
| Admiralty knot | Supported | Not supported |

Both platforms handle the core engineering and scientific units identically. Excel has more legacy and specialized units for compatibility with older workbooks. Always test specific unit combinations if cross-platform compatibility is required.

## Tips and Best Practices

1. **Use cell references for units**: Store unit abbreviations in cells and reference them in CONVERT formulas. This allows easy switching between unit systems by changing a single cell.

2. **Build unit conversion tables**: Create lookup tables with common conversions for your domain. Reference these for consistency and to document the unit abbreviations used.

3. **Handle temperature carefully**: Temperature conversions are not simple ratios. Converting temperature differences (delta T) requires different formulas than converting absolute temperatures.

4. **Combine with ROUND**: CONVERT often produces many decimal places. Use `=ROUND(CONVERT(...), 2)` to display appropriate precision for your application.

5. **Chain conversions when needed**: For units not directly supported, chain multiple CONVERT calls. For example, convert km/h to mph via m/s: `=CONVERT(CONVERT(100,"km","m")/"hr","m/s","mph")*3600`.

6. **Use binary prefixes for computing**: When dealing with computer storage, use "kibi", "mebi", "gibi" prefixes for accurate binary calculations instead of decimal "kilo", "mega", "giga".

7. **Document unit assumptions**: When sharing workbooks, clearly document which unit system is being used, especially for ambiguous units like "ton" (short, long, or metric).

8. **Validate with known conversions**: Test your formulas with known conversion values (e.g., 1 inch = 2.54 cm exactly) to ensure you're using the correct unit abbreviations.

## Related Functions

### Similar Functions

| Function | Description |
|----------|-------------|
| [[DOLLAR]] | Converts number to currency-formatted text (not unit conversion) |
| [[TEXT]] | Formats numbers with custom formatting including unit labels |
| [[VALUE]] | Converts text to numbers (can help prepare data for CONVERT) |

### Commonly Used With

- **[[ROUND]]** - Round converted values to appropriate decimal places
- **[[IF]]** - Apply conditional logic to choose conversion units dynamically
- **[[VLOOKUP]]** - Look up unit abbreviations from reference tables
- **[[INDEX/MATCH]]** - Retrieve conversion factors from custom tables
- **[[INDIRECT]]** - Build dynamic unit references from cell values
- **[[IFERROR]]** - Handle cases where unit conversion is not possible

## Official Documentation

- [Microsoft Excel CONVERT Documentation](https://support.microsoft.com/en-us/office/convert-function-d785bef1-808e-4aac-bdcd-666c810f9af2)
- [Google Sheets CONVERT Documentation](https://support.google.com/docs/answer/6055540)

## Version History

| Version | Date | Changes |
|---------|------|---------|
| Excel 2003 | 2003 | Available via Analysis ToolPak add-in |
| Excel 2007 | 2007 | Integrated into standard function library |
| Excel 2010 | 2010 | No changes |
| Excel 2013 | 2013 | Added binary prefixes for information units |
| Excel 2016 | 2016 | Added additional distance and volume units |
| Excel 2019 | 2019 | Added speed units (m/s, m/h, mph, kn) |
| Excel 365 | Ongoing | Current version with all units |
| Google Sheets | 2006 | Available since launch with core units |
| Google Sheets | 2015 | Added binary prefixes and speed units |
