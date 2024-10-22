# October 15, 2024

## Highlights
This release introduces a new mutation for user authentication, updates several queries for improved performance, and fixes critical bugs.

---

##  What's Changed
- [**NEW**] **AuthenticateUser**: A new mutation to handle user authentication, including login and token generation.
    - Example Usage:
      ```graphql
      mutation {
        authenticateUser(username: "user123", password: "password") {
          token
          user {
            id
            username
          }
        }
      }
      ```

- [**Update**] **GetUserProfile**: Improved the query to include additional user profile fields such as email and phone number.
    - Example of Change: Added `email` and `phone` fields to the response.
    - Example Usage:
      ```graphql
      query {
        getUserProfile(userId: "123") {
          id
          username
          email
          phone
        }
      }
      ```

- [**FIX**] **UpdateUserSettings**: Fixed a bug where the mutation did not correctly update user settings when certain fields were null.
    - Impact: Users can now update their settings without encountering errors when optional fields are left blank.

- [**DEPRECATE**] **OldAuthenticateUser**: This mutation is deprecated and will be removed in the next major release. Use `AuthenticateUser` instead.
    - Alternative: Use the new `AuthenticateUser` mutation for all authentication needs.

- [**REMOVED**] **GetUserByEmail**: This query has been removed due to privacy concerns and redundancy.
    - Reason for Removal: The functionality is now covered by `GetUserProfile` with enhanced security measures.
---

## Breaking Changes
- **UpdateUserSettings**: The mutation now requires the `userId` field to be non-null.
    - Migration Steps: Ensure that all calls to `UpdateUserSettings` include a valid `userId`.
    - Example of Old vs. New Usage:
      ```graphql
      # Old Usage
      mutation {
        updateUserSettings(settings: { theme: "dark" }) {
          success
        }
      }
  
      # New Usage
      mutation {
        updateUserSettings(userId: "123", settings: { theme: "dark" }) {
          success
        }
      }
      ```

---

## Additional Notes
This release focuses on enhancing security and performance. Please review the breaking changes section to ensure a smooth transition.
 