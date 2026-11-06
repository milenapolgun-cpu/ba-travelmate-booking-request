# Register User: dateOfBirth Field Requirements

**Endpoint:** `POST /auth/register`

**Change:** the `is_adult` field is removed from registration and replaced with the new required field `dateOfBirth`, which contains the user's exact date of birth.

## 1. Field Description

| Parameter | Value |
| --- | --- |
| Name | `dateOfBirth` |
| Data type | string |
| Format | `YYYY-MM-DD` (ISO 8601), e.g. `1995-04-23` |
| Required | Yes |
| Location | Request body (JSON) |

## 2. Validation Rules

**V1.** The field must be present in the request.

**V2.** The value cannot be an empty string (`""`).

**V3.** The value must match the `YYYY-MM-DD` format.

**V4.** The date must be a valid calendar date (e.g., `2023-02-30` or `1995-13-01` are not allowed).

**V5.** The date cannot be in the future.

**V6.** The user must be at least 18 years old on the registration date. The user is considered an adult on the day of their 18th birthday. Age is calculated using the full date (year, month and day), not just the difference between years.

*Example for registration date 2026-11-06:*

- `"dateOfBirth": "2008-11-06"`: the user turns 18 today, registration is allowed.
- `"dateOfBirth": "2008-11-07"`: the user turns 18 tomorrow, registration is rejected.

## 3. Validation Errors

All errors return **400 Bad Request** in `application/json`, using the same structure as the existing Register User validation error.

| Case | `code` | `message` |
| --- | --- | --- |
| Field is missing | `VALIDATION_ERROR` | `Date of birth is required` |
| Empty value | `VALIDATION_ERROR` | `Date of birth cannot be empty` |
| Invalid format | `VALIDATION_ERROR` | `Date of birth must be in YYYY-MM-DD format` |
| Invalid calendar date | `VALIDATION_ERROR` | `Date of birth must be a valid date` |
| Date is in the future | `VALIDATION_ERROR` | `Date of birth cannot be in the future` |
| User is under 18 | `VALIDATION_ERROR` | `User must be at least 18 years old` |

## 4. Examples

**Request**

```json
{
  "email": "newuser@example.com",
  "password": "SecurePass123!",
  "firstName": "New",
  "lastName": "User",
  "dateOfBirth": "1995-04-23",
  "phone": "+1-555-000-0000"
}
```

**Error response (400)**

```json
{
  "code": "VALIDATION_ERROR",
  "message": "Date of birth must be in YYYY-MM-DD format"
}
```
