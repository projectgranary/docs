---
description: Explanation of error codes in GRNRY APIs
---

# API Error Codes

All GRNRY APIs follow a unified, typed error model. See the following table for information about the different errors types.

| Error Type | HTTP Code | Example Error Message |
| :--- | :--- | :--- |
| `bad_parameter_value` | 400 | Parameter 'version' must be a natural number or 'latest' but is '-1'. |
| `bad_header_value` | 400 | Forwarded header verification failed. Cannot find 'example.com' in specified trusted hosts. |
| `authentication_error` | 401 | Authentication failed. |
| `entity_not_accessible` | 403 | Access forbidden due to missing roles. |
| `entity_not_found` | 404 | Harvester 'my-harvester' not found. |
| `entity_already_exists` | 409 | Harvester 'my-harvester' already exists. |
| `entity_in_use` | 424 | Cannot delete Event Type 'my-type', it is still being referenced by belts \[..., ..., ..\]. |
| `unexpected_error` | 500 | An unexpected error occurred. |

