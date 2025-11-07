# BUG-003: Invalid/Empty JSON Returns 500 Instead of 400

## Bug Summary

Invalid or empty JSON requests return HTTP 500 instead of HTTP 400.

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

**Justification**: Returning 500 (server error) for client input errors is wrong and misleading. Makes it look like a server problem when the issue is invalid client data. Can confuse developers and trigger false alerts in monitoring systems.

---

## Description

The API doesn't properly handle invalid or empty JSON request bodies. When empty JSON or malformed JSON is sent, the API throws HTTP 500 instead of returning HTTP 400.

This is critical because:

- **500 Internal Server Error** indicates a server-side problem
- **400 Bad Request** should be used for invalid client input
- Invalid/empty JSON is a client error, not a server error

---

## Test Cases

### Test 1: Empty JSON

**Request:**

```bash
curl -X POST "https://ec.europa.eu/taxation_customs/vies/rest-api/check-vat-number" \
  -H "Content-Type: application/json" \
  -d '{}'
```

**Response:**

```
HTTP/1.1 500 Internal Server Error
{
  "actionSucceed": false,
  "errorWrappers": [{
    "error": "INTERNAL_ERROR",
    "message": "An internal server error occurred"
  }]
}
```

❌ Returns 500 instead of 400

---

### Test 2: Trailing Comma

**Request:**

```bash
curl -X POST "https://ec.europa.eu/taxation_customs/vies/rest-api/check-vat-number" \
  -H "Content-Type: application/json" \
  -d '{"countryCode": "BE", "vatNumber": "0123456789",}'
```

**Response:**

```
HTTP/1.1 500 Internal Server Error
{
  "actionSucceed": false,
  "errorWrappers": [{
    "error": "INTERNAL_ERROR",
    "message": "An internal server error occurred"
  }]
}
```

❌ Returns 500 instead of 400

---

### Test 3: Unclosed Bracket

**Request:**

```bash
curl -X POST "https://ec.europa.eu/taxation_customs/vies/rest-api/check-vat-number" \
  -H "Content-Type: application/json" \
  -d '{"countryCode": "BE", "vatNumber": "0123456789"'
```

**Response:**

```
HTTP/1.1 500 Internal Server Error
{
  "actionSucceed": false,
  "errorWrappers": [{
    "error": "INTERNAL_ERROR",
    "message": "An internal server error occurred"
  }]
}
```

❌ Returns 500 instead of 400

---

### Test 4: Missing Quotes

**Request:**

```bash
curl -X POST "https://ec.europa.eu/taxation_customs/vies/rest-api/check-vat-number" \
  -H "Content-Type: application/json" \
  -d '{countryCode: "BE", vatNumber: "0123456789"}'
```

**Response:**

```
HTTP/1.1 500 Internal Server Error
{
  "actionSucceed": false,
  "errorWrappers": [{
    "error": "INTERNAL_ERROR",
    "message": "An internal server error occurred"
  }]
}
```

❌ Returns 500 instead of 400

---

## Expected vs Actual

**Expected**: HTTP 400 Bad Request with error details  
**Actual**: HTTP 500 Internal Server Error

---

## Impact

- Misleading error codes confuse developers
- False alerts in monitoring systems
- Can't distinguish client errors from server problems
- May expose internal error details (security concern)
- Violates REST API standards

**Frequency**: 100% reproducible

---

## Root Cause

API tries to parse JSON, hits parsing exception, doesn't catch it properly, returns HTTP 500 instead of HTTP 400.

**What should happen**: Catch JSON parsing exceptions, return HTTP 400 with clear error message, never expose internal server errors.

---

## Recommendations

1. Add JSON parsing exception handling at framework level
2. Catch all JSON parsing errors and return HTTP 400
3. Add request body validation before processing
4. Add proper exception handling middleware
5. Add automated tests for all JSON error scenarios
6. Update API documentation with expected error responses

---

**Test Cases**: TC-007 (Empty request body), TC-010 (Invalid JSON)  
**Date**: November 7, 2025
