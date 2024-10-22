# v3.2.0 - October 15, 2024

## Highlights
Major update introducing real-time webhooks, improved rate limiting, and enhanced authentication methods. This release focuses on improving scalability and real-time capabilities while maintaining backward compatibility for most existing integrations.

---

## What's Changed
- [**NEW**] **Real-time Webhooks**: Introduced a new webhooks system allowing instant notifications for resource changes and events.
    - Example:
      ```json
      {
        "webhook_url": "https://api.yourapp.com/webhooks",
        "events": ["user.created", "user.updated"],
        "version": "2.5",
        "secret": "whsec_..."
      }
      ```

- [**NEW**] **Enhanced Rate Limiting**: Improved rate limiting system with more granular controls and better header information.
    - Now includes `X-RateLimit-Reset-At` header with timestamp
    - Increased default rate limits from 1000 to 5000 requests per hour
    - Example Response Headers:
      ```
      X-RateLimit-Limit: 5000
      X-RateLimit-Remaining: 4995
      X-RateLimit-Reset-At: 2024-10-15T15:00:00Z
      ```

- [**FIX**] **Pagination Token Handling**: Fixed an issue where pagination tokens would occasionally expire prematurely for large result sets.
    - Previous behavior: Tokens expired after 5 minutes regardless of usage
    - New behavior: Tokens remain valid for the duration of pagination sequence

- [**DEPRECATE**] **Legacy Authentication**: The v1 authentication method using API keys without prefixes is now deprecated.
    - Alternative: Use the new authentication format `Bearer sk_live_...` for all requests
    - Will be removed in v3.0.0 (estimated Q2 2025)

- [**REMOVED**] **/v1/batch Endpoint**: The legacy batch processing endpoint has been removed as announced in v2.4.0.
    - Example: Use the new `/v2/bulk` endpoint instead:
      ```json
      {
        "operations": [
          {"method": "POST", "path": "/v2/users", "body": {...}},
          {"method": "POST", "path": "/v2/orders", "body": {...}}
        ]
      }
      ```

---

## Breaking Changes
- **Updated Response Format**: All API responses now include a `meta` object containing request-specific metadata.
    - Migration Steps:
        1. Update response parsing to handle the new structure
        2. Access response data through the `data` field
        3. Use `meta` field for pagination, rate limiting, and request information
    - Example:
      ```json
      {
        "meta": {
          "request_id": "req_123",
          "timestamp": "2024-10-15T10:30:00Z"
        },
        "data": {
          // Your actual response data
        }
      }
      ```

---

## Additional Notes
- All endpoints now support JSON:API specification for better consistency
- Documentation has been updated with new interactive examples
- Beginning January 2025, we will require TLS 1.3 for all API connections
- Please join our developer community on Discord for migration support