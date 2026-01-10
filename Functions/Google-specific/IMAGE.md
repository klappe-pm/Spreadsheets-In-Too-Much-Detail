---
categories:
  - spreadsheet-functions
subCategories:
  - sheets
topics:
  - google-specific
  - images
  - visualization
subTopics:
  - image-embedding
  - url-images
  - cell-images
  - visual-content
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
  - insert-image
  - embed-image
  - url-image
  - cell-image
tags:
  - google-sheets-only
  - images
  - visualization
  - url
  - web-content
  - formatting
---

# IMAGE

## Description

The **IMAGE function** is a Google Sheets-exclusive function that inserts an image into a cell by fetching it from a specified URL. This powerful function allows users to display images directly within spreadsheet cells, transforming static data into visually rich documents that can include product photos, logos, icons, charts, QR codes, flags, and any other web-accessible image. The function accepts a URL pointing to an image file and optional parameters controlling how the image is sized and positioned within the cell, making it invaluable for creating product catalogs, inventory lists, dashboards, and visual databases.

The IMAGE function supports common web image formats including JPEG, PNG, GIF (including animated GIFs), BMP, ICO, and SVG. The image URL must be publicly accessible—images behind authentication, on private networks, or requiring login cannot be displayed. Google Sheets caches images for performance, so changes to the source image may not appear immediately. The function offers four sizing modes: fit to cell while maintaining aspect ratio (default), stretch to fill the entire cell, use original image size, or specify custom dimensions. These options provide flexibility for different use cases, from thumbnail galleries to full-size product images.

One of the most creative uses of IMAGE is combining it with dynamic URL construction to create data-driven visualizations. For example, you can use Google Charts API URLs to generate charts, QR code API URLs to create QR codes from cell data, or flag API URLs to display country flags based on country codes. By concatenating cell values into URLs, each row can display a unique, automatically-generated image. This transforms Google Sheets from a simple data table into a dynamic visual application without requiring external tools or programming.

IMAGE is exclusively available in Google Sheets and has no direct equivalent function in Microsoft Excel. Excel users can insert images into cells using the Pictures feature (Insert > Pictures) or link images via the Insert Picture dialog, but there is no formula-based approach that dynamically loads images from URLs. Excel 365 introduced the IMAGE function in late 2022, but it works differently and has more limited availability. For cross-platform compatibility, worksheets using the IMAGE function should be converted to static images before sharing with Excel users.

## Syntax

> [!f(x)] IMAGE Syntax
>
> ```
> =IMAGE(url, [mode], [height], [width])
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `url` | Yes | The URL of the image to display. Must be a complete, publicly accessible URL starting with http:// or https://. Can be a string in quotes, a cell reference, or a formula that constructs a URL. The image format must be supported (JPEG, PNG, GIF, BMP, ICO, SVG). |
| `mode` | No | A number from 1-4 specifying how to size the image. 1 = Resize to fit cell, maintaining aspect ratio (default). 2 = Stretch to fill cell, ignoring aspect ratio. 3 = Display at original size (may be cropped if larger than cell). 4 = Use custom size specified by height and width parameters. |
| `height` | No | The height in pixels for the image. Only used when mode is 4. If specified without width, width defaults to automatic based on aspect ratio. |
| `width` | No | The width in pixels for the image. Only used when mode is 4. If specified without height, height defaults to automatic based on aspect ratio. |

### Return Value

Displays an image within the cell. The cell shows the image rather than text or a value. If the image cannot be loaded (invalid URL, network error, unsupported format), the cell displays an error or placeholder. The underlying value of the cell is the formula, not the image data.

## Examples

> [!f(x)] IMAGE Examples

### Example 1: Basic Image from URL
```
=IMAGE("https://www.google.com/images/branding/googlelogo/1x/googlelogo_color_272x92dp.png")
```
**Scenario:** Displaying a logo image from a direct URL
**Result:** The Google logo appears in the cell

**Explanation:** This basic example loads an image from a public URL using default sizing (mode 1), which fits the image to the cell while maintaining aspect ratio. Adjust the row height to see the full image.

---

### Example 2: Image Stretched to Fill Cell
```
=IMAGE("https://example.com/photo.jpg", 2)
```
**Scenario:** Making an image fill the entire cell regardless of proportions
**Result:** Image fills the cell completely, potentially distorted

**Explanation:** Mode 2 stretches the image to fill both the height and width of the cell. This can distort images if the cell proportions don't match the image, but ensures no empty space.

---

### Example 3: Original Size Image
```
=IMAGE("https://example.com/icon.png", 3)
```
**Scenario:** Displaying a small icon at its native resolution
**Result:** Image displays at original pixel dimensions

**Explanation:** Mode 3 shows the image at its original size. If the image is larger than the cell, it will be cropped. This is useful for icons or images where exact pixel dimensions matter.

---

### Example 4: Custom Sized Image
```
=IMAGE("https://example.com/photo.jpg", 4, 100, 150)
```
**Scenario:** Displaying an image at exactly 100 pixels tall and 150 pixels wide
**Result:** Image appears at specified dimensions

**Explanation:** Mode 4 with height and width parameters gives precise control over image dimensions. This is useful for creating uniform image sizes across a product catalog.

---

### Example 5: Image URL from Cell Reference
```
=IMAGE(A1)
```
**Scenario:** Cell A1 contains "https://example.com/product-image.jpg"
**Result:** Displays the image from the URL in A1

**Explanation:** Using a cell reference for the URL enables dynamic image display. Update the URL in A1, and the image updates automatically. This is the foundation for building image databases.

---

### Example 6: Constructing URL Dynamically for Product Images
```
=IMAGE("https://mystore.com/products/" & A2 & ".jpg")
```
**Scenario:** A2 contains the product SKU "SKU12345"
**Result:** Loads image from "https://mystore.com/products/SKU12345.jpg"

**Explanation:** Concatenating cell values into URLs creates data-driven image galleries. Each row can display its corresponding product image based on the SKU or ID.

---

### Example 7: QR Code Generation
```
=IMAGE("https://api.qrserver.com/v1/create-qr-code/?size=150x150&data=" & ENCODEURL(A1))
```
**Scenario:** A1 contains "https://example.com/page"
**Result:** Displays a QR code that links to the URL in A1

**Explanation:** Using a QR code API with cell data creates dynamic QR codes for each row. ENCODEURL ensures special characters in the data are properly handled.

---

### Example 8: Country Flag Display
```
=IMAGE("https://flagcdn.com/w80/" & LOWER(A1) & ".png")
```
**Scenario:** A1 contains "US" (country code)
**Result:** Displays the United States flag

**Explanation:** Flag CDN services provide flags by country code. This formula displays the appropriate flag for any country code, useful for international customer lists or regional reports.

---

### Example 9: Google Charts API Integration
```
=IMAGE("https://chart.googleapis.com/chart?cht=p3&chs=300x150&chd=t:" & B2 & "," & C2 & "&chl=Sales|Returns")
```
**Scenario:** B2 contains 75, C2 contains 25
**Result:** Displays a pie chart showing 75% Sales, 25% Returns

**Explanation:** The Google Charts API generates chart images from URL parameters. This enables mini-charts within cells based on spreadsheet data. Note: Google Charts Image API is deprecated but still functional.

---

### Example 10: Conditional Image Display
```
=IF(A1="Active", IMAGE("https://example.com/green-check.png"), IMAGE("https://example.com/red-x.png"))
```
**Scenario:** Display different icons based on status
**Result:** Shows green check if Active, red X otherwise

**Explanation:** Combining IMAGE with IF enables conditional image display, useful for visual status indicators, approval workflows, or any binary state representation.

---

### Example 11: Image with Error Handling
```
=IFERROR(IMAGE(A1), "No image available")
```
**Scenario:** A1 may contain a valid URL or be empty
**Result:** Shows image if valid, text message if not

**Explanation:** IFERROR prevents error displays when URLs are invalid, missing, or inaccessible. This creates a cleaner user experience in forms and data entry sheets.

---

### Example 12: Sparkline-Style Bar Using Image
```
=IMAGE("https://chart.googleapis.com/chart?cht=bhs&chs=100x20&chd=t:" & A1 & "&chco=4d89f9&chbh=10")
```
**Scenario:** A1 contains a value from 0-100
**Result:** Displays a horizontal bar chart representing the value

**Explanation:** Single-bar charts from the Charts API create visual progress indicators. Combined with data, each row shows a visual representation of its value.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#VALUE!` | Invalid URL format or empty URL | Ensure URL is complete with http:// or https://; check for typos |
| `#N/A` | Image could not be loaded | Verify URL is accessible; check if image requires authentication |
| `Image not displaying` | URL points to non-image content | Ensure URL directly links to an image file, not an HTML page |
| `Broken image icon` | Image server is down or URL changed | Update URL; consider hosting critical images on reliable service |
| `Image appears distorted` | Using mode 2 with mismatched proportions | Use mode 1 for aspect ratio preservation or adjust cell dimensions |
| `Image too small or cropped` | Cell size doesn't accommodate image | Adjust row height and column width; use mode 4 for specific dimensions |
| `#REF!` | Referenced cell for URL was deleted | Restore cell or update formula reference |
| `Image not updating` | Google Sheets caching old image | Add a cache-busting parameter like `&t=` & NOW() to URL (may cause refresh issues) |

## Use Cases

### [[Product Catalog with Images]]

**Scenario:** An e-commerce company maintains their product catalog in Google Sheets. Each product needs to display its image alongside the name, description, price, and inventory count. The marketing team updates product images on their CDN, and the spreadsheet should automatically reflect current images.

**Implementation:**
```
Product catalog structure:
Column A: Product SKU
Column B: Product Name
Column C: Image (formula)
Column D: Description
Column E: Price
Column F: Stock

Image formula (C2):
=IFERROR(IMAGE("https://cdn.mystore.com/products/" & A2 & ".jpg", 1), IMAGE("https://cdn.mystore.com/placeholder.jpg", 1))

Row height setting: 75 pixels for consistent thumbnail display

Alternative with category folders:
=IMAGE("https://cdn.mystore.com/" & G2 & "/" & A2 & ".jpg", 1)
where G2 contains the category folder name

Batch image display with ARRAYFORMULA:
Note: IMAGE doesn't work directly with ARRAYFORMULA
Use individual formulas or Apps Script for batch operations
```

**Business Application:** Product catalogs are significantly more useful when images are visible alongside product data. Sales teams can quickly identify products during calls, warehouse staff can visually verify items during picks, and marketing can review visual consistency across the catalog. The dynamic URL construction means new products automatically display images without formula updates—just upload the image with the correct SKU filename.

**Technical Details:** Use a consistent URL structure on your CDN (product ID as filename) to enable formula-based image loading. Set row heights uniformly for visual consistency. Consider creating a thumbnail version of each image for faster loading. The IFERROR wrapper ensures products without images show a placeholder rather than an error.

---

### [[Visual Dashboard with Dynamic Charts]]

**Scenario:** A sales manager needs a dashboard showing key metrics with visual representations. The dashboard should update automatically as underlying data changes, displaying mini-charts for each metric including sales trends, regional breakdowns, and target achievement gauges.

**Implementation:**
```
Dashboard structure:
A1: "Metric"        B1: "Value"    C1: "Visual"
A2: "Q1 Sales"      B2: 45000      C2: (chart image)
A3: "Q2 Sales"      B3: 52000      C3: (chart image)
A4: "Target %"      B4: 85         C4: (gauge image)

Progress bar (C2):
=IMAGE("https://chart.googleapis.com/chart?cht=bhs&chs=200x30&chd=t:" & ROUND(B2/1000) & "&chco=4285f4&chbh=20&chds=0,100", 1)

Gauge for percentage (C4):
=IMAGE("https://chart.googleapis.com/chart?cht=gom&chs=200x100&chd=t:" & B4 & "&chl=" & B4 & "%", 1)

Comparison bar chart:
=IMAGE("https://chart.googleapis.com/chart?cht=bvg&chs=200x150&chd=t:" & B2/1000 & "," & B3/1000 & "," & B4/1000 & "&chds=0,100&chl=Q1|Q2|Q3&chco=4285f4", 1)

Sparkline alternative (built-in):
=SPARKLINE(B2:B5, {"charttype","column";"color","blue"})
```

**Business Application:** Visual dashboards communicate performance more effectively than numbers alone. Executives can quickly assess status using color-coded gauges, and trends become immediately apparent with sparklines and bar charts. The IMAGE function extends beyond built-in SPARKLINE by enabling pie charts, gauges, and more complex visualizations.

**Technical Details:** Google Charts Image API (chart.googleapis.com) is deprecated but still functional. Consider alternative chart APIs for long-term solutions. Cache-busting parameters may be needed for real-time updates. Combine with SPARKLINE for native chart types and IMAGE for specialized visualizations.

---

### [[Employee Directory with Photos]]

**Scenario:** HR maintains an employee directory in Google Sheets that includes employee photos alongside contact information. The directory should display profile photos stored in Google Drive or a company server, with consistent sizing across all entries.

**Implementation:**
```
Directory structure:
Column A: Employee ID
Column B: Name
Column C: Photo
Column D: Department
Column E: Email
Column F: Phone
Column G: Photo URL (hidden)

Photo formula (C2):
=IF(ISBLANK(G2), IMAGE("https://company.com/assets/default-avatar.png", 4, 60, 60), IMAGE(G2, 4, 60, 60))

Google Drive images (G2 format):
Convert: https://drive.google.com/file/d/FILE_ID/view
To: https://drive.google.com/uc?export=view&id=FILE_ID

Formula with Drive ID extraction:
=IMAGE("https://drive.google.com/uc?export=view&id=" & REGEXEXTRACT(G2, "/d/([a-zA-Z0-9_-]+)"), 4, 60, 60)

Standardized row height: 65 pixels
Photo column width: 70 pixels
```

**Business Application:** Employee directories with photos help team members recognize colleagues, especially in large or distributed organizations. New employees can learn faces alongside names, and the directory becomes a go-to resource for finding and identifying team members. HR can update photos by simply changing URLs without modifying formulas.

**Technical Details:** Google Drive images require URL transformation to work with IMAGE. The file must be shared (at least "Anyone with the link can view"). Consider uploading photos to a more reliable hosting service for critical directories. Standardize photo dimensions and file formats for consistent display.

---

### [[Inventory Tracking with Visual Indicators]]

**Scenario:** A warehouse manager tracks inventory levels and wants visual status indicators showing stock status at a glance. Items should show green, yellow, or red indicators based on current stock versus reorder points, plus product images for identification.

**Implementation:**
```
Inventory structure:
Column A: SKU
Column B: Product Name
Column C: Product Image
Column D: Current Stock
Column E: Reorder Point
Column F: Status Icon

Product Image (C2):
=IMAGE("https://warehouse.company.com/images/" & A2 & ".jpg", 4, 50, 50)

Status Icon (F2):
=IF(D2>E2*1.5,
  IMAGE("https://company.com/icons/green-circle.png", 4, 20, 20),
  IF(D2>E2,
    IMAGE("https://company.com/icons/yellow-circle.png", 4, 20, 20),
    IMAGE("https://company.com/icons/red-circle.png", 4, 20, 20)))

Alternative using emoji-based URLs:
=IMAGE("https://emojipedia-us.s3.amazonaws.com/thumbs/120/" &
  IF(D2>E2*1.5, "green", IF(D2>E2, "yellow", "red")) & "-circle.png", 4, 20, 20)

Stock level bar:
=IMAGE("https://chart.googleapis.com/chart?cht=bhs&chs=100x15&chd=t:" &
  MIN(100, ROUND(D2/E2*50)) & "&chco=" &
  IF(D2>E2*1.5, "00ff00", IF(D2>E2, "ffff00", "ff0000")) & "&chbh=10", 1)
```

**Business Application:** Visual inventory indicators enable quick scanning for items needing attention. Red indicators immediately draw attention to stockouts or critical levels, reducing the time spent reviewing detailed numbers. Product images help warehouse staff visually verify items during picks and cycle counts, reducing errors.

**Technical Details:** Host status icon images on reliable infrastructure since they're used across many rows. Consider using Google Sheets' built-in conditional formatting for colored cells as an alternative to image icons. The chart-based progress bar provides a more nuanced view of stock levels relative to reorder points.

## Platform Differences

IMAGE is a **Google Sheets-exclusive function** with limited equivalent functionality in Microsoft Excel.

### Google Sheets
- **Availability:** Full support in all versions since early Google Sheets
- **URL Support:** Any publicly accessible HTTP/HTTPS image URL
- **Formats:** JPEG, PNG, GIF (animated), BMP, ICO, SVG
- **Sizing Modes:** Four flexible modes including custom dimensions
- **Caching:** Images are cached for performance

### Microsoft Excel
- **Native IMAGE Function:** Introduced in Excel 365 (late 2022), available to Microsoft 365 subscribers
- **Syntax Differences:** Excel's IMAGE uses different parameters and alt text support
- **Availability:** Not available in Excel 2019, 2016, or earlier
- **Alternative Methods:** Insert > Pictures for static images
- **Web Images:** Requires manual copy/paste or Power Query

### Key Differences Summary

| Feature | Google Sheets | Microsoft Excel |
|---------|---------------|-----------------|
| Formula-based Images | Yes (IMAGE) | Yes (365 only) |
| URL Support | Full HTTP/HTTPS | Full HTTP/HTTPS (365) |
| Sizing Modes | 4 modes | Different parameters |
| Animated GIF | Yes | Limited |
| Legacy Version Support | All versions | 365 only |
| Dynamic URL Construction | Full support | Full support (365) |

## Tips and Best Practices

1. **Use reliable image hosting:** Images from unreliable servers may fail to load or disappear. For critical images, use established CDNs (CloudFlare, AWS S3, Google Cloud Storage) or Google Drive with proper sharing settings.

2. **Optimize image file sizes:** Large images slow down spreadsheet loading and may timeout. Use web-optimized images (compressed JPEG for photos, PNG for graphics with transparency) sized appropriately for their cell display size.

3. **Standardize row heights for image cells:** Set consistent row heights (e.g., 75 pixels) for cells containing images to ensure uniform display. Use View > Freeze to keep headers visible while scrolling through image-heavy catalogs.

4. **Use mode 4 with explicit dimensions for consistency:** When building catalogs or directories, specify exact pixel dimensions to ensure all images display at the same size regardless of their original proportions.

5. **Implement error handling with IFERROR:** Wrap IMAGE in IFERROR to display placeholder images or text when URLs fail. This prevents error symbols from disrupting the visual layout.

6. **Consider SPARKLINE for native charts:** For simple bar charts, line charts, and basic visualizations, the built-in SPARKLINE function is more reliable than IMAGE with external chart APIs.

7. **Transform Google Drive URLs correctly:** Google Drive sharing URLs don't work directly with IMAGE. Convert them to the direct download format: `https://drive.google.com/uc?export=view&id=FILE_ID`.

8. **Test URL accessibility:** IMAGE requires publicly accessible URLs. Images behind logins, on private networks, or with restricted sharing settings won't display. Test URLs in an incognito browser window to verify accessibility.

## Related Functions

### Similar Functions

| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[SPARKLINE]] | Creates mini-charts within cells | For line, bar, column, or winloss charts without external APIs |
| [[HYPERLINK]] | Creates clickable links | When you want a link to an image rather than embedded display |
| [[IMPORTHTML]] | Imports tables or lists from web pages | For extracting data, not images, from web pages |
| [[ENCODEURL]] | URL-encodes text | For including cell data in image URLs safely |

### Commonly Used Together

**[[IFERROR]]** - Handle missing or broken images

*Display placeholder when image fails to load:*
```
=IFERROR(IMAGE(A1), IMAGE("https://example.com/placeholder.png"))
```
Ensures visual consistency even when some images are unavailable.

---

**[[CONCATENATE]] / [[Ampersand (&)]]** - Build dynamic URLs

*Construct image URL from cell data:*
```
=IMAGE("https://cdn.example.com/products/" & A1 & ".jpg")
```
Enables data-driven image galleries based on IDs or codes.

---

**[[ENCODEURL]]** - Safely encode URL parameters

*Include cell data in API URLs:*
```
=IMAGE("https://api.qrserver.com/v1/create-qr-code/?data=" & ENCODEURL(A1))
```
Handles special characters in data used within URLs.

---

**[[IF]]** - Conditional image display

*Show different images based on conditions:*
```
=IF(B1>50, IMAGE("https://example.com/high.png"), IMAGE("https://example.com/low.png"))
```
Enables status indicators and conditional visualizations.

---

**[[SPARKLINE]]** - Native mini-charts

*Combine with IMAGE for comprehensive dashboards:*
```
Cell C1: =SPARKLINE(A1:A10)
Cell D1: =IMAGE("https://chart.apis.google.com/chart?cht=gom&chd=t:" & B1)
```
Use SPARKLINE for supported chart types, IMAGE for others.

## Official Documentation

- **Google Sheets:** [IMAGE function](https://support.google.com/docs/answer/3093333)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Google Sheets | ~2010 | Original implementation |
| Google Sheets | 2014 | Added mode parameter for sizing control |
| Google Sheets | 2016 | Improved caching and loading performance |
| Google Sheets | 2018 | Enhanced SVG support |
| Google Sheets | 2020 | Improved error handling |
| Google Sheets | 2023 | Continued reliability improvements |
| Microsoft Excel | 2022 (365) | Excel introduced its own IMAGE function |

---

*Last updated: 2026-01-10*
