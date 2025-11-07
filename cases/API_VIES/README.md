# VIES API Testing - Bug Reports

Bug reports for the VIES Public VAT Validation API.

**API Documentation**: https://ec.europa.eu/assets/taxud/vow-information/swagger_publicVAT.yaml  
**Base URL**: https://ec.europa.eu/taxation_customs/vies/rest-api  
**Test Date**: November 7, 2025

---

## Summary of Found Bugs

| Bug ID  | Severity | Priority | Issue                                         | Status |
| ------- | -------- | -------- | --------------------------------------------- | ------ |
| BUG-002 | High     | High     | Invalid input returns 200 instead of 400      | New    |
| BUG-003 | High     | High     | Invalid/empty JSON returns 500 instead of 400 | New    |
| BUG-004 | Medium   | Medium   | Trader info not processed                     | New    |

---

## BUG-002: Invalid Input Returns 200 Instead of 400

### Summary

Invalid input (country code or requester info) returns HTTP 200 with error messages instead of HTTP 400.

### Test Cases

**Test 1: Invalid Country Code**

```bash
curl -X POST "https://ec.europa.eu/taxation_customs/vies/rest-api/check-vat-number" \
  -H "Content-Type: application/json" \
  -d '{"countryCode": "XX", "vatNumber": "123456789"}'
```

**Response:**

```
HTTP/1.1 200 OK
{
  "actionSucceed": false,
  "errorWrappers": [{
    "error": "INVALID_INPUT",
    "message": "Invalid country code or VAT number format"
  }]
}
```

❌ Returns 200 instead of 400

---

**Test 2: Invalid Requester Info**

```bash
curl -X POST "https://ec.europa.eu/taxation_customs/vies/rest-api/check-vat-number" \
  -H "Content-Type: application/json" \
  -d '{
    "countryCode": "BE",
    "vatNumber": "0123456789",
    "requester": {"countryCode": "XX", "vatNumber": "INVALID"}
  }'
```

**Response:**

```
HTTP/1.1 200 OK
{
  "actionSucceed": false,
  "errorWrappers": [{
    "error": "INVALID_REQUESTER_INFO",
    "message": "Invalid requester information provided"
  }]
}
```

❌ Returns 200 instead of 400

### Impact

- Can't use HTTP status codes for error handling
- Must parse response body to check for errors
- Breaks REST API standards

---

## BUG-003: Invalid/Empty JSON Returns 500 Instead of 400

### Summary

Invalid or empty JSON returns HTTP 500 instead of HTTP 400.

### Test Cases

**Test 1: Empty JSON**

```bash
curl -X POST "https://ec.europa.eu/taxation_customs/vies/rest-api/check-vat-number" \
  -H "Content-Type: application/json" \
  -d '{}'
```

❌ Returns 500 instead of 400

**Test 2: Trailing Comma**

```bash
curl -X POST "https://ec.europa.eu/taxation_customs/vies/rest-api/check-vat-number" \
  -H "Content-Type: application/json" \
  -d '{"countryCode": "BE", "vatNumber": "0123456789",}'
```

❌ Returns 500 instead of 400

**Test 3: Unclosed Bracket**

```bash
curl -X POST "https://ec.europa.eu/taxation_customs/vies/rest-api/check-vat-number" \
  -H "Content-Type: application/json" \
  -d '{"countryCode": "BE", "vatNumber": "0123456789"'
```

❌ Returns 500 instead of 400

**Test 4: Missing Quotes**

```bash
curl -X POST "https://ec.europa.eu/taxation_customs/vies/rest-api/check-vat-number" \
  -H "Content-Type: application/json" \
  -d '{countryCode: "BE", vatNumber: "0123456789"}'
```

❌ Returns 500 instead of 400

### Impact

- Misleading error codes
- False alerts in monitoring systems
- Can't distinguish client errors from server problems
- Security concern (may expose internal errors)

---

## BUG-004: Trader Info Not Processed

### Summary

Trader information is accepted but not processed. All trader fields return "NOT_PROCESSED".

### Test Case

```bash
curl -X POST "https://ec.europa.eu/taxation_customs/vies/rest-api/check-vat-number" \
  -H "Content-Type: application/json" \
  -d '{
    "countryCode": "BE",
    "vatNumber": "0123456789",
    "trader": {
      "name": "Test Company",
      "city": "Brussels",
      "postalCode": "1000"
    }
  }'
```

**Response:**

```
HTTP/1.1 200 OK
{
  "countryCode": "BE",
  "vatNumber": "0123456789",
  "valid": true,
  "name": "NOT_PROCESSED",
  "address": "NOT_PROCESSED",
  "traderName": "NOT_PROCESSED",
  "traderAddress": "NOT_PROCESSED"
}
```

❌ All trader fields return "NOT_PROCESSED"

### Impact

- Trader information feature doesn't work
- Approximate matching unavailable

---

## Test Summary

### Results

| Category       | Total | Passed | Failed | Bugs  |
| -------------- | ----- | ------ | ------ | ----- |
| Valid Requests | 3     | 3      | 0      | 0     |
| Invalid Input  | 3     | 0      | 3      | 3     |
| **Total**      | **6** | **3**  | **3**  | **3** |

### Critical Issues

1. **Wrong HTTP Status Codes**: All 3 bugs have wrong status codes (200/500 instead of 400)
2. **Non-functional Feature**: Trader info processing doesn't work
3. **Poor Error Handling**: Client errors returned as server errors

### Recommendations

1. Fix HTTP status codes for validation errors (return 400)
2. Add input validation before processing
3. Fix trader information processing
4. Add proper JSON parsing error handling
5. Update API documentation with expected status codes

---

**Reported by**: QA Engineer  
**Date**: November 7, 2025  
**Status**: Testing Complete - 3 Bugs Found
