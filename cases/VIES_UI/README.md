# BUG-001: Incomplete Translation After Language Switch

## Bug Summary
Partial or incomplete translation when switching from English to German (and likely other languages) on the VIES VAT Validation page. Some UI elements remain in English despite translation files being successfully loaded.

---

## Quick Info
- **Status**: New  
- **Severity**: Medium  
- **Priority**: High  
- **Date**: November 7, 2025
- **Type**: Localization/Translation Bug
- **Affected URL**: https://ec.europa.eu/taxation_customs/vies/#/vat-validation

---

## Summary
Translation files load successfully but are not fully applied to the UI when switching languages from English to German. Several critical UI elements remain in English, including disclaimer messages, navigation items, and validation messages.

---

## Visual Evidence

| Before | After | Issue |
|--------|-------|-------|
| ![English page](./screenshot_01_initial_page.png) | ![Mixed translation](./screenshot_03_translation_bug.png) | ![Untranslated elements](./screenshot_05_untranslated_elements.png) |
| Initial page in English | Page after language switch | Untranslated validation message |

**All Screenshots**:
- ✅ `screenshot_01_initial_page.png` - Initial page in English
- ✅ `screenshot_02_language_selector.png` - Language selection modal
- ✅ `screenshot_03_translation_bug.png` - Page after language switch (minimal translation)
- ✅ `screenshot_04_partial_translation.png` - Partial translation after interaction
- ✅ `screenshot_05_untranslated_elements.png` - Untranslated form validation
- ✅ `screenshot_06_final_state.png` - Final state overview

---

## Testing Environment
- **Application**: VIES on the Web v7.2.1
- **Test Date**: November 7, 2025
- **Browser**: Google Chrome 142.0.0.0
- **OS**: macOS 24.6.0

---

## Test Coverage
- ✅ Language switching functionality
- ✅ Translation file loading
- ✅ UI element translation verification
- ✅ Network request analysis
- ✅ Cross-language consistency

---

📋 [**Full Detailed Report**](./BUG-001_Incomplete_Translation_After_Language_Switch.md)

---

**Reported by**: QA Engineer  
**Date**: November 7, 2025  
**Status**: New

