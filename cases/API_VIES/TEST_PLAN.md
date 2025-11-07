# VIES API Test Plan

**Application**: VIES on the Web - Public VAT Validation API  
**API Documentation**: https://ec.europa.eu/assets/taxud/vow-information/swagger_publicVAT.yaml  
**Base URL**: https://ec.europa.eu/taxation_customs/vies/rest-api  
**Test Date**: November 7, 2025  
**Tester**: QA Engineer

---

## 1. Test Objectives

- Verify functionality of all VIES API endpoints
- Validate request/response formats according to Swagger specification
- Test error handling and edge cases
- Identify security vulnerabilities
- Document any bugs or inconsistencies

---

## 2. API Endpoints to Test

### 2.1 POST /check-vat-number

**Purpose**: Check VAT number validation for a specific country

**Test Cases**:

- TC-001: Valid VAT number validation (exact match)
- TC-002: Valid VAT number with requester information
- TC-003: Valid VAT number with trader information (approximate matching)
- TC-004: Invalid VAT number
- TC-005: Invalid country code
- TC-006: Missing required fields
- TC-007: Empty request body
- TC-008: Special characters in VAT number
- TC-009: SQL injection attempt
- TC-010: XSS attempt in fields
- TC-011: Very long VAT number (boundary test)
- TC-012: Null values in optional fields

---

## 3. Test Data

### Valid VAT Numbers (for testing):

- **Belgium (BE)**: BE0123456789
- **Germany (DE)**: DE123456789
- **France (FR)**: FR12345678901
- **Italy (IT)**: IT12345678901
- **Netherlands (NL)**: NL123456789B01
- **Spain (ES)**: ESX12345678

### Invalid Test Cases:

- Invalid country code: XX123456789
- Wrong format: BE123 (too short)
- Special characters: BE<script>alert(1)</script>
- SQL injection: BE' OR '1'='1

---

## 4. Expected Results

### Successful Validation (200):

```json
{
  "countryCode": "string",
  "vatNumber": "string",
  "requestDate": "datetime",
  "valid": true/false,
  "name": "string",
  "address": "string"
}
```

### Bad Request (400):

```json
{
  "actionSucceed": false,
  "errorWrappers": [
    {
      "error": "string",
      "message": "string"
    }
  ]
}
```

### Server Error (500):

```json
{
  "actionSucceed": false,
  "errorWrappers": [...]
}
```

---

## 5. Security Test Cases

- **Authentication**: Check if API requires authentication
- **Rate Limiting**: Test for rate limiting mechanisms
- **Input Validation**: Special characters, SQL injection, XSS
- **Error Messages**: Check for information disclosure
- **HTTPS**: Verify secure connection
- **CORS**: Check CORS headers

---

## 7. Negative Test Cases

- Missing required fields
- Invalid JSON format
- Unsupported HTTP methods (GET on POST endpoints)
- Invalid content-type headers
- Extremely large payloads
- Unicode and special characters

---

## 8. Test Environment

**Tools**:

- curl (command-line HTTP client)
- jq (JSON processor)
- Browser Developer Tools

**Prerequisites**:

- Internet connection
- Access to VIES API (public)

---

## 9. Test Execution Plan

1. **Phase 1**: Basic functionality tests (TC-001 to TC-004)
2. **Phase 2**: Edge cases and validation (TC-005 to TC-012)
3. **Phase 3**: Test service endpoint (TC-013 to TC-016)
4. **Phase 4**: Status endpoint (TC-017 to TC-020)
5. **Phase 5**: Security tests
6. **Phase 6**: Performance tests
7. **Phase 7**: Bug reporting and documentation

---

## 10. Success Criteria

- All valid requests return 200 with correct data
- Invalid requests return appropriate error codes (400)
- Security tests pass without vulnerabilities
- API documentation matches actual behavior
- Response times are acceptable (< 3 seconds)
- No critical or high severity bugs found

---

## 11. Risks and Assumptions

**Risks**:

- API may be rate-limited
- Test data may not exist in production database
- API availability depends on member states

**Assumptions**:

- API is publicly accessible
- No authentication required
- Swagger documentation is up-to-date
