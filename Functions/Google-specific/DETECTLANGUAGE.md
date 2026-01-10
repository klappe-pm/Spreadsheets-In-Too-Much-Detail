---
categories:
  - spreadsheet-functions
subCategories:
  - sheets
topics:
  - google-specific
  - language-detection
  - localization
subTopics:
  - language-identification
  - multilingual-content
  - text-analysis
  - internationalization
dateCreated: '2025-08-17'
dateRevised: '2026-01-10'
aliases:
  - detect-language
  - identify-language
  - language-identification
  - language-detector
tags:
  - google-sheets-only
  - language-detection
  - localization
  - multilingual
  - text-analysis
  - web-services
---

# DETECTLANGUAGE

## Description

The **DETECTLANGUAGE function** is a Google Sheets-exclusive function that analyzes text and returns a two-letter ISO 639-1 language code identifying the language of the input. This function leverages Google's language detection algorithms to automatically identify the language of text content, making it invaluable for processing multilingual data, routing content for translation, and analyzing international datasets. DETECTLANGUAGE examines the characters, words, and patterns in the input text to determine the most likely language, returning codes such as "en" for English, "es" for Spanish, "zh" for Chinese, or "ar" for Arabic.

The function works by sending the input text to Google's language detection service, which uses machine learning models trained on vast multilingual datasets. For accurate detection, the function generally requires a minimum amount of text—single words may produce unreliable results, while sentences or paragraphs provide much higher accuracy. DETECTLANGUAGE can distinguish between languages that share alphabets (like Spanish and Portuguese, or Dutch and German) by analyzing word patterns, common letter combinations, and linguistic markers. The function returns the detected language as a lowercase two-letter code, making it easy to use in conditional formulas, lookups, and integration with GOOGLETRANSLATE.

One important consideration is that DETECTLANGUAGE may struggle with very short text, mixed-language content, or languages with limited training data. When text is too short or ambiguous, the function may return an incorrect language code or occasionally return an error. For romanized versions of non-Latin languages (like pinyin for Chinese or romaji for Japanese), detection may fail or incorrectly identify the text as another Latin-script language. Despite these limitations, DETECTLANGUAGE provides a powerful automated solution for language identification that would otherwise require manual review or complex external integrations.

DETECTLANGUAGE is exclusively available in Google Sheets and has no direct equivalent in Microsoft Excel. Excel users must rely on external solutions such as Azure Cognitive Services Language Detection API, third-party add-ins, or custom VBA/Python scripts that call language detection services. This makes DETECTLANGUAGE a distinctive Google Sheets capability, particularly valuable for organizations processing international content or managing multilingual communications within their spreadsheets.

## Syntax

> [!f(x)] DETECTLANGUAGE Syntax
>
> ```
> =DETECTLANGUAGE(text_or_range)
> ```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `text_or_range` | Yes | The text to analyze for language detection. Can be a string in quotes, a cell reference containing text, or a range of cells. When a range is provided, returns detection results for each cell. Longer text (at least 20+ characters) produces more reliable detection. |

### Return Value

Returns a lowercase two-letter ISO 639-1 language code representing the detected language (e.g., "en", "es", "fr", "de", "zh", "ja", "ar"). If the language cannot be determined, may return "und" (undefined) or an error. For empty cells, returns an empty string or error depending on context.

## Examples

> [!f(x)] DETECTLANGUAGE Examples

### Example 1: Detecting English Text
```
=DETECTLANGUAGE("Hello, how are you today?")
```
**Scenario:** Identifying a simple English sentence
**Result:** "en"

**Explanation:** The function analyzes the text and correctly identifies it as English based on common English words and patterns. This straightforward example demonstrates the basic functionality.

---

### Example 2: Detecting Spanish Text
```
=DETECTLANGUAGE("Buenos dias, como esta usted?")
```
**Scenario:** Identifying Spanish text
**Result:** "es"

**Explanation:** Spanish-specific words like "Buenos," "dias," and "usted" clearly signal Spanish. The function distinguishes Spanish from other Romance languages like Portuguese or Italian.

---

### Example 3: Detecting Chinese Text
```
=DETECTLANGUAGE("Chinese characters here")
```
**Scenario:** Identifying Simplified Chinese text
**Result:** "zh"

**Explanation:** Chinese characters are distinctive and easily identified. The function returns "zh" for Chinese without distinguishing between Simplified and Traditional variants in the standard return code.

---

### Example 4: Detecting Japanese Text
```
=DETECTLANGUAGE("Japanese text here")
```
**Scenario:** Identifying Japanese text containing hiragana/katakana
**Result:** "ja"

**Explanation:** Japanese is identified by its unique mix of hiragana, katakana, and kanji characters. The presence of hiragana or katakana strongly signals Japanese rather than Chinese.

---

### Example 5: Detecting from Cell Reference
```
=DETECTLANGUAGE(A1)
```
**Scenario:** Cell A1 contains "Guten Morgen, wie geht es Ihnen?"
**Result:** "de"

**Explanation:** The function works with cell references, enabling dynamic language detection as cell contents change. This is the most common usage pattern in practical workflows.

---

### Example 6: Using with GOOGLETRANSLATE
```
=GOOGLETRANSLATE(A1, DETECTLANGUAGE(A1), "en")
```
**Scenario:** Automatically translating any language to English
**Result:** English translation of whatever text is in A1

**Explanation:** Nesting DETECTLANGUAGE inside GOOGLETRANSLATE creates an automated translation pipeline that handles any input language. This is the most powerful combination of these two functions.

---

### Example 7: Conditional Logic Based on Language
```
=IF(DETECTLANGUAGE(A1)="en", "No translation needed", GOOGLETRANSLATE(A1, "auto", "en"))
```
**Scenario:** Only translate text that isn't already in English
**Result:** Original text if English, translated text otherwise

**Explanation:** This pattern prevents unnecessary translation operations and avoids potential translation artifacts when content is already in the target language.

---

### Example 8: Language-Based Routing
```
=SWITCH(DETECTLANGUAGE(A1),
  "en", "Route to English team",
  "es", "Route to Spanish team",
  "fr", "Route to French team",
  "de", "Route to German team",
  "Route to translation service")
```
**Scenario:** Assigning customer inquiries to language-specific support teams
**Result:** Routing instruction based on detected language

**Explanation:** SWITCH combined with DETECTLANGUAGE enables automated content routing. The default case handles languages without dedicated teams.

---

### Example 9: Batch Language Detection with ARRAYFORMULA
```
=ARRAYFORMULA(IF(A2:A100="","",DETECTLANGUAGE(A2:A100)))
```
**Scenario:** Detecting languages for an entire column of text
**Result:** Language codes for each row with content

**Explanation:** ARRAYFORMULA enables batch processing. The IF wrapper prevents errors on empty cells. This is essential for processing large datasets.

---

### Example 10: Language Distribution Analysis
```
=COUNTIF(B:B, "en") & " English, " & COUNTIF(B:B, "es") & " Spanish, " & COUNTIF(B:B, "fr") & " French"
```
**Scenario:** After detecting languages in column B, counting submissions by language
**Result:** "245 English, 128 Spanish, 67 French"

**Explanation:** Once languages are detected, standard counting functions analyze the distribution. This helps understand the linguistic composition of datasets.

---

### Example 11: Detecting Arabic Text
```
=DETECTLANGUAGE("Arabic text here")
```
**Scenario:** Identifying Arabic right-to-left text
**Result:** "ar"

**Explanation:** Arabic script is distinctive and easily detected. The function correctly identifies Arabic regardless of text direction settings in the spreadsheet.

---

### Example 12: Handling Short or Ambiguous Text
```
=IF(LEN(A1)<20, "Text too short", DETECTLANGUAGE(A1))
```
**Scenario:** Warning when text may be too short for reliable detection
**Result:** Warning message for short text, language code for longer text

**Explanation:** Very short text may produce unreliable detection. This pattern warns users when input length is likely insufficient for accurate results.

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `#VALUE!` | Empty cell or invalid input | Ensure cell contains text; check for formula errors in source cell |
| `#N/A` | Service unavailable or text unrecognizable | Check internet connection; verify text is not garbled |
| `Returns incorrect language` | Text too short or ambiguous | Provide more text context (20+ characters); avoid single words |
| `Returns "und"` | Language undefined/undetectable | Text may be in unsupported language, mixed languages, or nonsensical |
| `Unexpected language for romanized text` | Romanized versions of non-Latin languages | Detection works best with native scripts; romanized text may misidentify |
| `Same result for similar languages` | Insufficient text to distinguish dialects | Provide more text; accept that closely related languages may overlap |
| `Error with special characters only` | Text contains only symbols, numbers, or emojis | Ensure text includes actual language content |
| `Inconsistent results on refresh` | Detection algorithm updates or service variability | Cache results by copying as values for stable results |

## Use Cases

### [[Multilingual Customer Support Triage]]

**Scenario:** A global company receives support tickets through a Google Form. Tickets arrive in various languages, and the support team needs to automatically route tickets to the appropriate language-specific queue before agents review them.

**Implementation:**
```
Support ticket processing sheet:
Column A: Ticket ID (from form)
Column B: Customer Email (from form)
Column C: Ticket Content (from form)
Column D: Detected Language
Column E: Support Queue
Column F: Priority

Language Detection (D2):
=IF(C2="", "", DETECTLANGUAGE(C2))

Queue Assignment (E2):
=IF(D2="", "",
  SWITCH(D2,
    "en", "english-support@company.com",
    "es", "spanish-support@company.com",
    "fr", "french-support@company.com",
    "de", "german-support@company.com",
    "pt", "portuguese-support@company.com",
    "ja", "japan-support@company.com",
    "zh", "china-support@company.com",
    "translation-needed@company.com"))

Dashboard metrics:
English tickets: =COUNTIF(D:D, "en")
Spanish tickets: =COUNTIF(D:D, "es")
Translation queue: =COUNTIF(E:E, "translation-needed@company.com")
```

**Business Application:** Instead of requiring all customers to submit tickets in English or manually reviewing each ticket for language, DETECTLANGUAGE automates routing. Support teams see tickets in their language, response times decrease, and customer satisfaction improves. Tickets in unsupported languages are flagged for translation before routing.

**Technical Details:** Connect the form response sheet to queue sheets using IMPORTRANGE or Apps Script automation. Set up email notifications based on queue assignments. Track average response time by language to ensure equitable service levels across regions.

---

### [[Content Localization Audit]]

**Scenario:** A multinational corporation maintains thousands of documents across regional SharePoint sites and Google Drive folders. The content team needs to audit which languages are present in each repository to plan localization efforts and identify gaps in multilingual coverage.

**Implementation:**
```
Content audit sheet:
Column A: Document ID
Column B: Document Title
Column C: Sample Text (first 500 characters)
Column D: Detected Language
Column E: Expected Language (from metadata)
Column F: Language Match

Language Detection (D2):
=DETECTLANGUAGE(C2)

Match Check (F2):
=IF(D2=E2, "Match", "MISMATCH - Review")

Summary dashboard:
Total documents: =COUNTA(A:A)-1
English content: =COUNTIF(D:D, "en")
Spanish content: =COUNTIF(D:D, "es")
Mismatched documents: =COUNTIF(F:F, "*MISMATCH*")

Language coverage matrix:
=QUERY(A:F, "SELECT E, COUNT(A) WHERE E IS NOT NULL GROUP BY E LABEL COUNT(A) 'Document Count'")
```

**Business Application:** Large organizations often lose track of which content exists in which languages. DETECTLANGUAGE provides automated verification that documents are in their expected languages, identifies mislabeled content, and quantifies localization gaps. This supports prioritization of translation budgets and vendor management.

**Technical Details:** Extract sample text from documents using Google Docs API or third-party tools. Use DETECTLANGUAGE on samples rather than full documents for efficiency. Flag mismatches for human review—some may be intentional (like English technical terms in localized documents).

---

### [[Academic Research Language Analysis]]

**Scenario:** A linguistics research team is studying language use patterns on social media. They have collected thousands of posts and need to categorize them by language, analyze the distribution, and identify multilingual content for further study.

**Implementation:**
```
Research data structure:
Column A: Post ID
Column B: Username (anonymized)
Column C: Post Content
Column D: Detected Language
Column E: Confidence Indicator
Column F: Multilingual Flag

Language Detection (D2):
=DETECTLANGUAGE(C2)

Confidence based on length (E2):
=IF(LEN(C2)>100, "High",
  IF(LEN(C2)>50, "Medium",
    IF(LEN(C2)>20, "Low", "Very Low")))

Multilingual check (F2):
=IF(DETECTLANGUAGE(LEFT(C2, LEN(C2)/2)) <> DETECTLANGUAGE(RIGHT(C2, LEN(C2)/2)), "Possible", "Unlikely")

Statistical summary:
=QUERY(A:F, "SELECT D, COUNT(A), AVG(LEN(C)) WHERE D IS NOT NULL GROUP BY D ORDER BY COUNT(A) DESC")

Language pair analysis (for multilingual posts):
=COUNTIFS(D:D, "es", F:F, "Possible") & " Spanish posts with code-switching"
```

**Business Application:** Linguistic research often requires large-scale language categorization that would be impractical to do manually. DETECTLANGUAGE provides automated first-pass categorization, and the confidence indicator helps researchers prioritize verification efforts. The multilingual detection helps identify code-switching behavior for sociolinguistic analysis.

**Technical Details:** The multilingual check compares detection on the first and second halves of posts—different results suggest code-switching. Export categorized data for statistical analysis in R or Python. Create pivot tables on detected language for distribution visualization.

---

### [[International SEO Content Verification]]

**Scenario:** An SEO agency manages websites in multiple languages. They need to verify that pages on country-specific domains are actually written in the expected language, identify accidental language mismatches, and audit translation completeness.

**Implementation:**
```
SEO audit structure:
Column A: URL
Column B: Target Country
Column C: Expected Language
Column D: Page Title (extracted)
Column E: Detected Language
Column F: Status

Language mapping:
.de domain -> "de" expected
.fr domain -> "fr" expected
.es domain -> "es" expected
.jp domain -> "ja" expected

Detection (E2):
=DETECTLANGUAGE(D2)

Status check (F2):
=IF(ISBLANK(E2), "Detection failed",
  IF(E2=C2, "Correct",
    IF(E2="en", "Not translated", "Wrong language")))

Audit summary:
Correct language: =COUNTIF(F:F, "Correct")
Not translated: =COUNTIF(F:F, "Not translated")
Wrong language: =COUNTIF(F:F, "Wrong language")

Priority issues:
=QUERY(A:F, "SELECT A, C, E, F WHERE F = 'Wrong language' OR F = 'Not translated'")
```

**Business Application:** International SEO requires content in the correct language for each regional domain. Google and users expect .de domains to have German content. DETECTLANGUAGE automates verification across potentially thousands of pages, identifying translation gaps and misconfigurations that could hurt search rankings and user experience.

**Technical Details:** Extract page titles using IMPORTXML or web scraping tools. Compare detected language to expected language based on domain or URL structure. Create alerts for new "Not translated" pages to prioritize translation backlog.

## Platform Differences

DETECTLANGUAGE is **exclusively available in Google Sheets** and has no native equivalent in Microsoft Excel or other spreadsheet applications.

### Google Sheets
- **Availability:** Full support in all versions since early Google Sheets
- **API Integration:** Direct connection to Google's language detection services
- **Language Support:** Supports 100+ languages using ISO 639-1 codes
- **Real-time Processing:** Requires internet connection; processes via Google servers

### Microsoft Excel
- **Native Support:** No built-in language detection function exists
- **Alternatives:**
  - **Azure Cognitive Services Language Detection API:** Cloud-based detection requiring API setup
  - **Power Automate/Power Query:** Can connect to language detection APIs with custom configuration
  - **VBA Scripts:** Custom code can call REST APIs for language detection
  - **Microsoft Translator Add-in:** Limited language detection capabilities
  - **Python integration:** Use langdetect or other libraries via xlwings or Office Scripts

### Key Differences Summary

| Feature | Google Sheets | Microsoft Excel |
|---------|---------------|-----------------|
| Native Language Detection | Yes (DETECTLANGUAGE) | No |
| Formula-based Detection | Yes | No |
| Batch Processing | Yes (with ARRAYFORMULA) | Custom solution required |
| Supported Languages | 100+ | Varies by solution |
| Setup Complexity | None (built-in) | Requires API setup |
| Cost | Free (within limits) | May require paid API access |

## Tips and Best Practices

1. **Provide sufficient text length:** DETECTLANGUAGE works best with at least 20-50 characters of text. Single words or very short phrases may produce unreliable or incorrect results. When possible, use complete sentences or paragraphs for detection.

2. **Handle edge cases with validation:** Before calling DETECTLANGUAGE, check for empty cells, very short text, or non-text content. Use formulas like `=IF(LEN(A1)<20, "Too short", DETECTLANGUAGE(A1))` to flag potentially unreliable detections.

3. **Use native scripts when possible:** Language detection is most accurate when text uses its native script (Chinese characters for Chinese, Arabic script for Arabic). Romanized text (pinyin, romaji) may be misidentified as a Latin-script language like English or Spanish.

4. **Combine with GOOGLETRANSLATE for automation:** The most powerful pattern is `=GOOGLETRANSLATE(A1, DETECTLANGUAGE(A1), "en")` which automatically translates any language to your target language without manual language specification.

5. **Cache results for stability:** Detection results may vary slightly over time as Google updates its algorithms. For reports or analyses requiring stable results, copy and paste values (Ctrl+Shift+V) to preserve detected languages.

6. **Account for mixed-language content:** Text containing multiple languages (code-switching, quoted foreign phrases) may return unpredictable results. The function typically returns the dominant language but may not accurately represent multilingual content.

7. **Create lookup tables for language names:** The function returns codes like "en" or "es" rather than "English" or "Spanish." Create a reference sheet mapping codes to full language names for user-friendly reports: `=VLOOKUP(D1, LanguageCodes!A:B, 2, FALSE)`.

8. **Test with your specific content types:** Detection accuracy varies by language pair, text type, and length. Test with samples representative of your actual data before deploying detection in production workflows.

## Related Functions

### Similar Functions

| Function | Description | When to Use Instead |
|----------|-------------|---------------------|
| [[GOOGLETRANSLATE]] | Translates text between languages | When you need to translate text, not just identify its language |
| [[LEN]] | Returns the length of text | For validating text length before detection |
| [[IF]] | Conditional logic | For routing based on detected language |
| [[SWITCH]] | Multiple condition matching | For mapping language codes to actions |

### Commonly Used Together

**[[GOOGLETRANSLATE]]** - Translate after detecting language

*Automatic translation of any language to English:*
```
=GOOGLETRANSLATE(A1, DETECTLANGUAGE(A1), "en")
```
The most common combination—detect source language and pass it to GOOGLETRANSLATE.

---

**[[IF]]** - Conditional logic based on language

*Different actions for different languages:*
```
=IF(DETECTLANGUAGE(A1)="en", "Process directly", "Send to translation")
```
Route content based on whether it needs translation.

---

**[[SWITCH]]** - Route to multiple language teams

*Assign content to language-specific handlers:*
```
=SWITCH(DETECTLANGUAGE(A1), "en", "Team A", "es", "Team B", "fr", "Team C", "Translation Service")
```
More elegant than nested IF statements for multiple language handling.

---

**[[ARRAYFORMULA]]** - Batch process language detection

*Detect languages for an entire column:*
```
=ARRAYFORMULA(IF(A2:A="", "", DETECTLANGUAGE(A2:A)))
```
Efficiently process large datasets without copying formulas to each row.

---

**[[COUNTIF]]** - Analyze language distribution

*Count content by language:*
```
=COUNTIF(B:B, "es")
```
After detection, count how many items are in each language for analysis and reporting.

---

**[[VLOOKUP]]** - Convert codes to language names

*Display friendly language names:*
```
=VLOOKUP(DETECTLANGUAGE(A1), LanguageCodes!A:B, 2, FALSE)
```
Transform "es" into "Spanish" for user-friendly reports.

## Official Documentation

- **Google Sheets:** [DETECTLANGUAGE function](https://support.google.com/docs/answer/3093278)

## Version History

| Platform | Version Introduced | Notes |
|----------|-------------------|-------|
| Google Sheets | ~2010 | Original implementation |
| Google Sheets | 2016 | Improved detection accuracy with neural models |
| Google Sheets | 2018 | Expanded language support |
| Google Sheets | 2020 | Enhanced detection for less common languages |
| Google Sheets | 2023 | Continued accuracy improvements |
| Google Sheets | 2025 | Additional language variants supported |

---

*Last updated: 2026-01-10*
