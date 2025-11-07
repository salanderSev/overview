# QA Manual Testing Portfolio

Welcome to my manual QA testing portfolio. Here are my pet projects showcasing testing across different platforms and technologies.

---

## 🐛 Featured Bug Reports

### BUG-001: Incomplete Translation After Language Switch

**Application**: VIES (VAT Information Exchange System) - UI Testing  
**Severity**: Medium  
**Priority**: High  
**Status**: New  
**Date**: November 7, 2025

**Summary**: Translation files load successfully but are not fully applied to the UI when switching languages from English to German. Several critical UI elements remain in English, including disclaimer messages, navigation items, and validation messages.

**Testing Scope**:

- Localization and translation functionality
- Network request analysis
- UI element translation verification
- Cross-language consistency
- User experience for non-English speakers

**Key Findings**:

- ✅ Translation files successfully loaded (200 OK status)
- ✅ Language preference correctly saved in localStorage
- ❌ Disclaimer messages remain hardcoded in English
- ❌ Form validation messages not translated
- ❌ Navigation items partially untranslated
- ❌ Missing translation keys for accessibility labels

**Technical Analysis**:

- Verified network requests: `/rest-api/translations?lang=de` → Status 200
- Confirmed translation files contain correct German translations
- Identified hardcoded HTML content preventing translation
- Detected lazy translation application requiring user interaction

**Visual Evidence**: 6 detailed screenshots documenting the issue from initial page load through language selection to final mixed-content state

📋 **[Full Detailed Report](./cases/VIES_UI/BUG-001_Incomplete_Translation_After_Language_Switch.md)**

**Environment**:

- Application: VIES on the Web v7.2.1
- URL: https://ec.europa.eu/taxation_customs/vies/#/vat-validation
- Browser: Google Chrome 142.0.0.0
- OS: macOS 24.6.0

---

### VIES API Testing - Summary

**Application**: VIES Public VAT Validation API  
**Status**: Testing Complete  
**Date**: November 7, 2025  
**Bugs Found**: 3

**API Documentation**: https://ec.europa.eu/assets/taxud/vow-information/swagger_publicVAT.yaml  
**Base URL**: https://ec.europa.eu/taxation_customs/vies/rest-api

**Summary**: Comprehensive API testing revealed 3 critical bugs related to HTTP status code handling, input validation, and functional issues. All bugs violate REST API standards and impact error handling capabilities.

**Bugs Found**:

1. **BUG-002**: Invalid input returns 200 instead of 400 (High Severity)

   - Invalid country code returns 200 OK with error message
   - Invalid requester info returns 200 OK with error message
   - Breaks REST API standards for error handling

2. **BUG-003**: Invalid/empty JSON returns 500 instead of 400 (High Severity)

   - Empty JSON, malformed JSON, trailing commas all return 500
   - Client errors incorrectly reported as server errors
   - Security concern (may expose internal errors)

3. **BUG-004**: Trader info not processed (Medium Severity)
   - Trader information accepted but all fields return "NOT_PROCESSED"
   - Approximate matching feature non-functional

**Test Results**:

- Valid Requests: 3/3 passed
- Invalid Input Tests: 3/3 failed (bugs found)
- **Total**: 6 test cases, 3 bugs identified

**Key Issues**:

- ❌ Wrong HTTP status codes (200/500 instead of 400)
- ❌ Non-functional feature (trader info processing)
- ❌ Poor error handling (client errors as server errors)

📋 **[Full API Testing Report](./cases/API_VIES/README.md)**

---

## 🛠️ Testing Tools & Methodologies

- **Test Management**: Jira
- **API Testing**: curl, Postman, Swagger/OpenAPI
- **UI Testing**: Browser Developer Tools, Network Analysis
- **Documentation**: Test cases, checklists, bug reports, test plans
- **Testing Types**: Functional, UI/UX, API, Regression, Smoke, Edge case testing, Security testing

---

## 📚 Key Skills Demonstrated

✅ Detailed bug report writing with reproduction steps  
✅ Cross-platform testing (Web, Mobile, Desktop)  
✅ API testing (REST APIs, HTTP status codes, error handling)  
✅ UI testing (localization, translation, user experience)  
✅ Network condition testing and analysis  
✅ Edge case and error scenario identification  
✅ Security testing (input validation, error message disclosure)  
✅ Performance and usability testing awareness  
✅ Test planning and documentation

---

## 📋 Test Cases

Detailed test case documentation for all projects:

- **[View Test Cases](./cases/)**

---

## 🎓 Certifications

- **QA Manual Testing Diploma** - [View Certificate](./diploma%20QA%20en.pdf)

---

## 📧 Contact

Looking forward to discussing QA opportunities and testing strategies!
