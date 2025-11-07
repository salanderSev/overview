# BUG-002: Invalid Input Returns 200 Instead of 400

## Bug Summary

When invalid input is provided (invalid country code or invalid requester information), the API returns HTTP 200 with error messages instead of HTTP 400.

---

## Environment

- **API Endpoint**: `POST /check-vat-number`
- **Base URL**: `https://ec.europa.eu/taxation_customs/vies/rest-api`
- **Test Date**: November 7, 2025

---

## Severity & Priority

- **Severity**: High
- **Priority**: High
- **Type**: API Error Handling Bug

**Justification**: Wrong HTTP status codes break REST API standards. Developers can't rely on status codes for error handling and must parse response bodies instead.

---

## Description

The API doesn't validate input before processing. When invalid data is sent (invalid country code or invalid requester info), it processes the request and returns HTTP 200 with an error in the response body, instead of rejecting with HTTP 400.

This breaks REST API standards: 200 should mean success, 400 should mean client errors.

---

## Test Cases

### Test Case 1: Invalid Country Code

**Request:**

```bash
curl -X POST "https://ec.europa.eu/taxation_customs/vies/rest-api/check-vat-number" \
  -H "Content-Type: application/json" \
  -d '{
    "countryCode": "XX",
    "vatNumber": "123456789"
  }'
```

**Response:**

```
HTTP/1.1 200 OK

{
  "actionSucceed": false,
  "errorWrappers": [
    {
      "error": "INVALID_INPUT",
      "message": "Invalid country code or VAT number format"
    }
  ]
}
```

**Issue**: Returns 200 instead of 400 ❌

---

### Test Case 2: Invalid Requester Information

**Request:**

```bash
curl -X POST "https://ec.europa.eu/taxation_customs/vies/rest-api/check-vat-number" \
  -H "Content-Type: application/json" \
  -d '{
    "countryCode": "BE",
    "vatNumber": "0123456789",
    "requester": {
      "countryCode": "XX",
      "vatNumber": "INVALID"
    }
  }'
```

**Response:**

```
HTTP/1.1 200 OK

{
  "actionSucceed": false,
  "errorWrappers": [
    {
      "error": "INVALID_REQUESTER_INFO",
      "message": "Invalid requester information provided"
    }
  ]
}
```

**Issue**: Returns 200 instead of 400 ❌

---

## Expected vs Actual

**Expected**: HTTP 400 Bad Request with error details  
**Actual**: HTTP 200 OK with error in response body

---

## Impact

- Can't use HTTP status codes for error handling
- Must parse response body to check for errors
- Breaks REST API standards
- Causes issues with monitoring tools and API gateways

**Frequency**: 100% reproducible

---

## Root Cause

API accepts requests without validation, processes them, then returns business logic errors with HTTP 200.

**What should happen**: Validate input first, return HTTP 400 immediately if validation fails.

---

## Recommendations

1. Add input validation before processing
2. Return HTTP 400 for all validation errors
3. Keep error messages in response body
4. Add automated tests for validation scenarios
5. Update API documentation with expected status codes

---

**Test Cases**: TC-005 (Invalid country code), TC-008 (Invalid requester info)  
**Date**: November 7, 2025
