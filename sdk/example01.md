# v2.5.0 - October 15, 2024

## Highlights
This release introduces a new feature for asynchronous processing, enhances the logging mechanism, and fixes several bugs to improve stability.

---

## What's Changed
- [**NEW**] **Async Processing**: Added support for asynchronous processing to improve performance in high-load scenarios.
    - Example:
      ```java
      AsyncProcessor processor = new AsyncProcessor();
      processor.processAsync(data, result -> {
          System.out.println("Processing complete: " + result);
      });
      ```

- [**NEW**] **Enhanced Logging**: Improved the logging mechanism to provide more detailed and configurable logs.
    - Example:
      ```java
      Logger logger = Logger.getLogger("MyApp");
      logger.setLevel(Level.DEBUG);
      logger.debug("This is a debug message");
      ```

- [**FIX**] **Connection Leak**: Fixed a connection leak issue that occurred when closing database connections improperly.
    - Impact: This fix ensures that all database connections are properly closed, preventing resource exhaustion.

- [**DEPRECATE**] **Old Authentication Method**: Deprecated the `authenticate(String username, String password)` method. Use `authenticate(Credentials credentials)` instead.
    - Example: Use `authenticate(new Credentials(username, password))` instead of `authenticate(username, password)` as the latter will be removed in version 2.0.0.

- [**REMOVED**] **Legacy API Support**: Removed support for the legacy API that was deprecated in version 1.0.0.
    - Reason: The legacy API has been replaced by a more efficient and secure API.

---

## Breaking Changes
- **Database Connection Handling**: Changed the way database connections are managed, which may affect existing implementations.
    - Migration Steps:
      ```java
      // Old way
      Connection conn = DriverManager.getConnection(url, user, pass);
      // New way
      try (Connection conn = DriverManager.getConnection(url, user, pass)) {
          // Use connection
      }
      ```

---

## Dependency Updates
- **Apache Commons IO**: Upgraded `Apache Commons IO` from version `2.8.0` to `2.11.0`.

---

## Links
- **API Version**: Link to API version 1.2.0
- **SDK Reference Docs**: Link to SDK documentation

---

## Additional Notes
- Minimum Java version requirement increased to Java 11
- Spring Boot 2.x support will be deprecated in the next major release
- The async client is currently in beta and includes optional support for Project Reactor
- For users experiencing issues with the new builder patterns, we've provided a temporary compatibility layer in `com.example.sdk.compat`