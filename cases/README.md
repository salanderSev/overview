# QA Test Cases and Bug Reports

This directory contains bug reports and testing evidence for the VIES (VAT Information Exchange System) application.

---

## Bug Reports Overview

### UI Testing

#### BUG-001: Incomplete Translation After Language Switch

**Status**: New  
**Severity**: Medium  
**Priority**: High  
**Date**: November 7, 2025

**Summary**: Translation files load successfully but are not fully applied to the UI when switching languages from English to German. Several critical UI elements remain in English, including disclaimer messages, navigation items, and validation messages.

**Affected URL**: https://ec.europa.eu/taxation_customs/vies/#/vat-validation

📁 [**Full Report**](./BUG-001_Translation_Issue/)

---

### API Testing

#### VIES API - Public VAT Validation API

**Status**: Testing Complete  
**Date**: November 7, 2025

**Summary**: Testing of the VIES Public VAT Validation API revealed 3 bugs related to error handling, input validation, and functional issues.

**API Documentation**: https://ec.europa.eu/assets/taxud/vow-information/swagger_publicVAT.yaml  
**Base URL**: https://ec.europa.eu/taxation_customs/vies/rest-api

**Bugs Found**:

- BUG-002: Invalid input returns 200 instead of 400 (High) - covers invalid country code and invalid requester info
- BUG-003: Invalid/empty JSON returns 500 instead of 400 (High) - covers empty JSON and malformed JSON
- BUG-004: Trader info not processed (Medium)

📁 [**Full Report**](./API_VIES/)

---

## Testing Environment

### UI Testing

- **Application**: VIES on the Web v7.2.1
- **Browser**: Google Chrome 142.0.0.0
- **OS**: macOS 24.6.0

### API Testing

- **Testing Tool**: curl 8.1.2
- **OS**: macOS 24.6.0

### Test Date

November 7, 2025

---

## Test Coverage Summary

### UI Testing

- ✅ Language switching functionality
- ✅ Translation file loading
- ✅ UI element translation verification
- ✅ Network request analysis
- ✅ Cross-language consistency

### API Testing

- ✅ Basic functionality tests
- ✅ Input validation tests
- ✅ Error handling tests
- ✅ Edge case tests
- ✅ Security tests (input validation)

---

## Notes

- All screenshots are stored in PNG format at screen resolution
- Bug reports follow the standard template with severity, steps to reproduce, expected/actual results, and technical analysis
- API test logs include full curl commands and HTTP response details
- Each bug report is organized in its own directory for better structure
