# QA Manual Testing Portfolio

Welcome to my manual QA testing portfolio. Here are my pet projects showcasing testing across different platforms and technologies.

---

## 🐛 Featured Bug Report

### BUG-001: Incomplete Translation After Language Switch
**Application**: VIES (VAT Information Exchange System)  
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

📋 **[Full Detailed Report](./cases/BUG-001_Incomplete_Translation_After_Language_Switch.md)**

**Environment**:
- Application: VIES on the Web v7.2.1
- URL: https://ec.europa.eu/taxation_customs/vies/#/vat-validation
- Browser: Google Chrome 142.0.0.0
- OS: macOS 24.6.0

---

## 🛠️ Testing Tools & Methodologies

- **Test Management**: Jira
- **API Testing**: Postman, Swagger/OpenAPI
- **Documentation**: Test cases, checklists, bug reports
- **Testing Types**: Functional, UI/UX, Regression, Smoke, Edge case testing

---

## 📚 Key Skills Demonstrated

✅ Detailed bug report writing with reproduction steps  
✅ Cross-platform testing (Web, Mobile, Desktop)  
✅ API and UI testing  
✅ Network condition testing  
✅ Edge case and error scenario identification  
✅ Performance and usability testing awareness  

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
