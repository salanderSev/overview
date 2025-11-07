# BUG-004: Trader Info Not Processed - All Fields Return NOT_PROCESSED

## Bug Summary

When trader information is provided in the VAT validation request for approximate matching, all trader-related fields in the response return "NOT_PROCESSED" instead of being processed and validated.

---

## Environment

- **API Endpoint**: `POST /check-vat-number`
- **Base URL**: `https://ec.europa.eu/taxation_customs/vies/rest-api`
- **API Documentation**: https://ec.europa.eu/assets/taxud/vow-information/swagger_publicVAT.yaml
- **Test Date**: November 7, 2025

---

## Severity & Priority

- **Severity**: Medium
- **Priority**: Medium
- **Type**: Functional Bug

**Justification**: The trader information feature doesn't seem to work. While the API accepts trader information in the request, it doesn't process it, making this feature unusable. This affects users who need approximate matching functionality.

---

## Description

The VIES API endpoint `/check-vat-number` supports an optional `trader` object in the request body for approximate matching (when exact VAT number match is not found). However, when trader information is provided, the API does not process it and all trader-related fields in the response return "NOT_PROCESSED".

This suggests that:

1. The trader information processing logic is not implemented, or
2. The trader information processing is disabled, or
3. There is a bug preventing trader information from being processed

---

## Steps to Reproduce

1. Send a POST request to `https://ec.europa.eu/taxation_customs/vies/rest-api/check-vat-number`
2. Include trader information in the request body:
   ```json
   {
     "countryCode": "BE",
     "vatNumber": "0123456789",
     "trader": {
       "name": "Test Company",
       "city": "Brussels",
       "postalCode": "1000",
       "street": "Test Street"
     }
   }
   ```
3. Observe the response for trader-related fields

**Expected Result**:

- HTTP Status Code: **200 OK**
- Response should contain processed trader information or indicate that approximate matching was attempted
- Trader fields should show actual values or indicate why matching failed

**Actual Result**:

- HTTP Status Code: **200 OK**
- All trader-related fields in response contain "NOT_PROCESSED":
  ```json
  {
    "countryCode": "BE",
    "vatNumber": "0123456789",
    "requestDate": "2025-11-07T10:00:00Z",
    "valid": true,
    "name": "NOT_PROCESSED",
    "address": "NOT_PROCESSED",
    "traderName": "NOT_PROCESSED",
    "traderAddress": "NOT_PROCESSED",
    "traderCity": "NOT_PROCESSED",
    "traderPostalCode": "NOT_PROCESSED"
  }
  ```

---

## Test Case Reference

- **Test Case**: TC-009 (Test 9: Trader info not processed)
- **Test Plan**: TEST_PLAN.md Section 2.1, TC-003

---

## Request Details

### Request

```bash
curl -X POST "https://ec.europa.eu/taxation_customs/vies/rest-api/check-vat-number" \
  -H "Content-Type: application/json" \
  -d '{
    "countryCode": "BE",
    "vatNumber": "0123456789",
    "trader": {
      "name": "Test Company",
      "city": "Brussels",
      "postalCode": "1000",
      "street": "Test Street"
    }
  }'
```

### Response

```json
HTTP/1.1 200 OK
Content-Type: application/json

{
  "countryCode": "BE",
  "vatNumber": "0123456789",
  "requestDate": "2025-11-07T10:00:00Z",
  "valid": true,
  "name": "NOT_PROCESSED",
  "address": "NOT_PROCESSED",
  "traderName": "NOT_PROCESSED",
  "traderAddress": "NOT_PROCESSED",
  "traderCity": "NOT_PROCESSED",
  "traderPostalCode": "NOT_PROCESSED"
}
```

---

## Impact Assessment

**Affected Users**: Users who need approximate matching functionality when exact VAT number match is not available

**Business Impact**:

- Trader information feature is non-functional
- Users cannot use approximate matching to find VAT numbers
- Feature appears to be implemented in API but not working
- May cause confusion for API consumers expecting this functionality

**Frequency**: 100% reproducible with trader information in request

---

## Root Cause Analysis

Possible causes:

1. **Feature not implemented**: The trader information processing logic might not be fully implemented
2. **Feature disabled**: The feature might be disabled in the current API version
3. **Backend issue**: The service handling trader information might be unavailable or misconfigured
4. **Data processing error**: There might be an error in the trader information processing pipeline that causes it to fail silently

**What should happen**:

- Process trader information when provided
- Try to do approximate matching using trader details
- Return actual trader information if a match is found
- Return appropriate error or status if matching fails or isn't supported

---

## Recommendations

### What to check:

1. Check if the trader information processing service is running and configured correctly
2. Check API documentation - is the trader feature listed as available?
3. Check server logs for errors related to trader information processing
4. Test with different trader information combinations

### Possible fixes:

1. **If feature not implemented**:

   - Implement trader information processing logic
   - Add approximate matching algorithm
   - Update API documentation

2. **If feature disabled**:

   - Re-enable trader information processing
   - Fix any configuration issues
   - Update API documentation

3. **If backend issue**:
   - Fix backend service connectivity
   - Resolve configuration issues
   - Add proper error handling and logging

---

## Additional Test Cases

Test with various trader information combinations:

- Valid trader information with valid VAT number
- Valid trader information with invalid VAT number
- Partial trader information (only name, only address, etc.)
- Trader information for different countries
- Trader information with special characters

---

## Questions for Development Team

1. Is trader information processing feature intended to be available?
2. Is there a specific configuration required to enable this feature?
3. Are there any known limitations or restrictions for trader information processing?
4. Is this feature available for all member states or only specific ones?

---

**Date**: November 7, 2025
