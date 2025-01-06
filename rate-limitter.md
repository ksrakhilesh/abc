### Rate Limiting System

#### Problem Statement

Implement a **rate limiting system** to prevent API abuse by limiting the number of requests a user can make within different time windows.

#### Core Requirements

1. Track requests at user and endpoint level
2. Support different time windows (per second, minute, hour).
3. Efficient memory usage (cleanup old data when no longer needed).
4. O(1) lookup time for checking rate limits.

#### Example Use Cases

1. `/api/search`: Max 5 requests per second

   - **Scenario**: User A makes 6 rapid requests.
     - First 5 requests should succeed.
     - The 6th request should fail.
     - After 1 second, the counter should reset, allowing new requests.

2. `/api/upload`: Max 10 requests per minute
   - **Scenario**: User B uploads 11 files within 30 seconds.
     - First 10 requests should succeed.
     - The 11th request should fail.
     - After 1 minute from the first request, the counter should reset.

---

### Interfaces

#### RateLimitRule

Defines a rate limit rule for an endpoint.

```typescript
interface RateLimitRule {
  endpoint: string; // The API endpoint (e.g., '/api/search')
  limit: number; // Maximum number of requests allowed in the time window
  windowMs: number; // Time window in milliseconds (e.g., 1000 for 1 second)
}
```

#### RateLimitResult

Result returned when checking if a request is allowed.

```typescript
interface RateLimitResult {
  isAllowed: boolean; // Whether the request is allowed
  remainingLimit: number; // Number of requests remaining in the current window
  resetTime: Date; // When the current time window will reset
}
```

---

### Main Class

#### RateLimiter

The main class you need to implement.

```typescript
class RateLimiter {
  /**
   * Initialize the rate limiter with a list of rules.
   *
   * @param rules List of rate limit rules to enforce
   *
   * Example:
   * const limiter = new RateLimiter([
   *   { endpoint: '/api/search', limit: 5, windowMs: 1000 },
   *   { endpoint: '/api/upload', limit: 10, windowMs: 60000 }
   * ]);
   */
  constructor(rules: RateLimitRule[]) {
    // TODO: Initialize your data structures
  }

  /**
   * Check if a request should be allowed based on rate limits.
   *
   * @param userId Unique identifier for the user making the request
   * @param endpoint The API endpoint being accessed
   * @returns Result indicating if the request is allowed
   *
   * Example:
   * const result = limiter.checkLimit('user123', '/api/search');
   * if (result.isAllowed) {
   *   // Allow request
   * } else {
   *   // Return 429 Too Many Requests
   * }
   */
  checkLimit(userId: string, endpoint: string): RateLimitResult {
    // TODO: Implement rate limiting logic
  }
}
```

---

### Example Test Cases

#### 1. Basic Rate Limiting

```typescript
const limiter = new RateLimiter([
  { endpoint: "/api/search", limit: 5, windowMs: 1000 },
]);

// Should allow 5 requests
for (let i = 0; i < 5; i++) {
  const result = limiter.checkLimit("user1", "/api/search");
  console.log(result.isAllowed); // true
  console.log(result.remainingLimit); // 4, 3, 2, 1, 0
}

// Should deny the 6th request
const result = limiter.checkLimit("user1", "/api/search");
console.log(result.isAllowed); // false
console.log(result.remainingLimit); // 0
```

#### 2. Window Reset

```typescript
// Wait 1 second
await new Promise((resolve) => setTimeout(resolve, 1000));

// Should allow requests again
const result = limiter.checkLimit("user1", "/api/search");
console.log(result.isAllowed); // true
console.log(result.remainingLimit); // 4
```

---

### Constraints

1. The implementation should efficiently handle cleanup of old data to minimize memory usage.
2. Ensure O(1) lookup time for checking and updating rate limits.
3. Consider edge cases:
   - No rule exists for an endpoint.
   - Invalid or empty `userId`.
   - Requests made exactly at the boundary of the time window.

---

### Notes

- Focus on implementing the `checkLimit` method efficiently.
- Test thoroughly with different scenarios and edge cases.
