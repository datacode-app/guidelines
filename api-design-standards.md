# API Design Standards

## Purpose
Well-designed APIs are intuitive, consistent, and future-proof. This guide establishes standards for REST API design, ensuring our APIs are developer-friendly and maintainable.

## Core Principles

1. **Developer Experience First** - APIs should be intuitive and predictable
2. **Consistency** - Similar endpoints should work similarly
3. **Versioning** - APIs evolve; plan for change from day one
4. **Security** - Every endpoint must be authenticated and authorized
5. **Documentation** - APIs are only as good as their documentation

---

## REST Fundamentals

### HTTP Methods

Use HTTP methods according to their semantic meaning:

| Method | Usage | Idempotent | Safe |
|--------|-------|------------|------|
| **GET** | Retrieve resource(s) | ✅ Yes | ✅ Yes |
| **POST** | Create new resource | ❌ No | ❌ No |
| **PUT** | Replace entire resource | ✅ Yes | ❌ No |
| **PATCH** | Partial update of resource | ❌ No* | ❌ No |
| **DELETE** | Remove resource | ✅ Yes | ❌ No |

*PATCH can be idempotent depending on implementation

### Resource Naming

```
✅ Good Resource Names:
GET    /api/v1/users                    # Collection
GET    /api/v1/users/123                # Single resource
GET    /api/v1/users/123/orders         # Nested collection
POST   /api/v1/users                    # Create
PUT    /api/v1/users/123                # Replace
PATCH  /api/v1/users/123                # Update
DELETE /api/v1/users/123                # Delete

❌ Bad Resource Names:
GET    /api/v1/getUsers                 # Verb in URL
POST   /api/v1/users/create             # Unnecessary action
GET    /api/v1/user/123                 # Inconsistent (singular vs plural)
POST   /api/v1/users/delete/123         # Wrong method + action in URL
```

### Naming Conventions

```
✅ Use:
- Plural nouns for collections: /users, /orders, /products
- Lowercase with hyphens: /order-items, /shipping-addresses
- Nested resources: /users/123/orders/456

❌ Avoid:
- Verbs: /getUser, /createOrder
- camelCase or snake_case in URLs: /orderItems, /order_items
- File extensions: /users.json
- Trailing slashes: /users/123/
```

---

## URL Structure

### Base URL

```
https://api.example.com/v1/...
│      │               │   │
│      │               │   └── Resource path
│      │               └────── Version
│      └────────────────────── Subdomain for API
└───────────────────────────── Protocol (always HTTPS)
```

### Resource Hierarchy

```
# Shallow nesting (preferred - max 2 levels)
GET /users/123/orders

# Deep nesting (avoid)
❌ GET /users/123/orders/456/items/789/reviews

# Better: Flat structure with filters
✅ GET /reviews?item_id=789&order_id=456&user_id=123
```

### Query Parameters

```
# Filtering
GET /users?status=active&role=admin

# Sorting
GET /users?sort=-created_at,name
# - prefix for descending, no prefix for ascending

# Pagination
GET /users?page=2&limit=20
GET /users?offset=40&limit=20

# Field selection (sparse fieldsets)
GET /users?fields=id,name,email

# Search
GET /users?q=john
GET /products?search=laptop&category=electronics

# Expansion (include related resources)
GET /orders?expand=user,items
```

---

## Versioning

### URL Versioning (Recommended)

```
✅ Version in URL
GET /api/v1/users
GET /api/v2/users

Pros: Clear, easy to route, easy to cache
Cons: Pollutes URL space
```

### Header Versioning (Alternative)

```
GET /api/users
Accept: application/vnd.api+json; version=1

Pros: Clean URLs
Cons: Less visible, harder to test in browser
```

### Version Strategy

```
# Major versions for breaking changes
v1 -> v2 (breaking: response format changed)

# Keep old versions running for transition period
# Sunset policy: 6 months after new version release

# Communicate changes
- Changelog
- Migration guide
- Deprecation warnings in responses
```

### Deprecation Headers

```
# Warn clients about upcoming changes
HTTP/1.1 200 OK
Deprecation: true
Sunset: Sat, 31 Dec 2024 23:59:59 GMT
Link: <https://docs.example.com/api/v2>; rel="successor-version"
```

---

## Request Format

### Request Body (JSON)

```json
// ✅ Good: Clear, structured
POST /api/v1/users
{
  "email": "user@example.com",
  "name": "John Doe",
  "role": "member",
  "preferences": {
    "notifications": true,
    "theme": "dark"
  }
}

// ❌ Bad: Flat, unclear
{
  "user_email": "user@example.com",
  "user_name": "John Doe",
  "user_role": "member",
  "pref_notifications": true,
  "pref_theme": "dark"
}
```

### Field Naming

```
Use snake_case for JSON fields (consistent with databases):

✅ Good:
{
  "user_id": "123",
  "first_name": "John",
  "created_at": "2024-03-16T10:30:00Z"
}

Alternative (if using JavaScript frontend):
camelCase is acceptable if consistent:
{
  "userId": "123",
  "firstName": "John",
  "createdAt": "2024-03-16T10:30:00Z"
}

Pick one and be consistent across all APIs!
```

### Date/Time Format

```
✅ Use ISO 8601 (UTC)
{
  "created_at": "2024-03-16T10:30:00Z",
  "scheduled_for": "2024-03-17T14:00:00Z"
}

Include timezone if needed:
{
  "event_time": "2024-03-16T10:30:00-08:00"
}

❌ Avoid:
{
  "created_at": "03/16/2024",           // Ambiguous format
  "timestamp": 1710584400,              // Unix timestamp (not readable)
  "date": "March 16, 2024 10:30 AM"     // Human format (hard to parse)
}
```

---

## Response Format

### Standard Response Structure

```json
// ✅ Success Response (200, 201)
{
  "data": {
    "id": "123",
    "name": "John Doe",
    "email": "john@example.com"
  },
  "meta": {
    "timestamp": "2024-03-16T10:30:00Z",
    "request_id": "abc-123-def"
  }
}

// ✅ Collection Response
{
  "data": [
    { "id": "1", "name": "User 1" },
    { "id": "2", "name": "User 2" }
  ],
  "meta": {
    "total": 100,
    "page": 1,
    "per_page": 20,
    "total_pages": 5
  },
  "links": {
    "self": "/api/v1/users?page=1",
    "next": "/api/v1/users?page=2",
    "last": "/api/v1/users?page=5"
  }
}

// ✅ Error Response (4xx, 5xx)
{
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "Invalid input data",
    "details": [
      {
        "field": "email",
        "message": "Email is required",
        "code": "REQUIRED_FIELD"
      },
      {
        "field": "age",
        "message": "Must be at least 18",
        "code": "MIN_VALUE"
      }
    ]
  },
  "meta": {
    "timestamp": "2024-03-16T10:30:00Z",
    "request_id": "abc-123-def"
  }
}
```

### HTTP Status Codes

**Success (2xx)**
```
200 OK              - GET, PUT, PATCH successful
201 Created         - POST successful (resource created)
204 No Content      - DELETE successful or update with no content
```

**Client Errors (4xx)**
```
400 Bad Request     - Invalid request format/syntax
401 Unauthorized    - Authentication required or failed
403 Forbidden       - Authenticated but not authorized
404 Not Found       - Resource doesn't exist
405 Method Not Allowed - Wrong HTTP method
409 Conflict        - Resource conflict (e.g., duplicate email)
422 Unprocessable Entity - Validation error
429 Too Many Requests - Rate limit exceeded
```

**Server Errors (5xx)**
```
500 Internal Server Error - Generic server error
502 Bad Gateway     - Upstream service error
503 Service Unavailable - Temporary unavailability
504 Gateway Timeout - Upstream timeout
```

### Error Response Examples

```json
// Validation Error (422)
{
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "Request validation failed",
    "details": [
      {
        "field": "email",
        "message": "Email format is invalid",
        "code": "INVALID_FORMAT"
      }
    ]
  }
}

// Authentication Error (401)
{
  "error": {
    "code": "UNAUTHORIZED",
    "message": "Invalid or expired access token"
  }
}

// Not Found (404)
{
  "error": {
    "code": "RESOURCE_NOT_FOUND",
    "message": "User with ID 123 not found"
  }
}

// Rate Limit (429)
{
  "error": {
    "code": "RATE_LIMIT_EXCEEDED",
    "message": "Too many requests. Please try again later.",
    "retry_after": 60
  }
}
```

---

## Pagination

### Offset-Based (Simple)

```
# Request
GET /api/v1/users?page=2&limit=20
GET /api/v1/users?offset=20&limit=20

# Response
{
  "data": [...],
  "meta": {
    "total": 100,
    "page": 2,
    "per_page": 20,
    "total_pages": 5
  },
  "links": {
    "first": "/api/v1/users?page=1&limit=20",
    "prev": "/api/v1/users?page=1&limit=20",
    "self": "/api/v1/users?page=2&limit=20",
    "next": "/api/v1/users?page=3&limit=20",
    "last": "/api/v1/users?page=5&limit=20"
  }
}
```

### Cursor-Based (For Large/Real-time Data)

```
# Request
GET /api/v1/messages?cursor=eyJpZCI6MTIzfQ&limit=20

# Response
{
  "data": [...],
  "meta": {
    "has_more": true
  },
  "links": {
    "next": "/api/v1/messages?cursor=eyJpZCI6MTQzfQ&limit=20"
  }
}

Pros: Efficient for large datasets, consistent results
Cons: Can't jump to specific page
```

---

## Filtering, Sorting, Searching

### Filtering

```
# Simple filters (exact match)
GET /users?status=active
GET /users?status=active&role=admin

# Range filters
GET /orders?min_amount=100&max_amount=500
GET /events?start_date=2024-01-01&end_date=2024-12-31

# Array filters (OR logic)
GET /products?category=electronics,books
GET /users?id=1,2,3

# Complex filters (use query language)
GET /users?filter=age:gt:18,status:eq:active
```

### Sorting

```
# Single field
GET /users?sort=created_at          # Ascending
GET /users?sort=-created_at         # Descending (- prefix)

# Multiple fields
GET /users?sort=-created_at,name    # Created desc, then name asc
```

### Searching

```
# Full-text search
GET /products?q=wireless+headphones
GET /users?search=john

# Scoped search
GET /products?q=laptop&fields=name,description
```

---

## Authentication & Authorization

### Authentication

```
# Bearer Token (Recommended)
GET /api/v1/users
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...

# API Key (For service-to-service)
GET /api/v1/data
X-API-Key: your-api-key-here
```

### Authorization

```
# Check permissions for every request
# Return appropriate error if unauthorized

403 Forbidden
{
  "error": {
    "code": "FORBIDDEN",
    "message": "Insufficient permissions to access this resource"
  }
}
```

---

## Rate Limiting

### Headers

```
HTTP/1.1 200 OK
X-RateLimit-Limit: 1000          # Max requests per window
X-RateLimit-Remaining: 995       # Requests left in window
X-RateLimit-Reset: 1710584400    # Unix timestamp when limit resets
```

### Rate Limit Exceeded

```
HTTP/1.1 429 Too Many Requests
Retry-After: 60

{
  "error": {
    "code": "RATE_LIMIT_EXCEEDED",
    "message": "API rate limit exceeded",
    "retry_after": 60
  }
}
```

---

## Idempotency

### Idempotency Keys (For POST)

```
# Client sends idempotency key
POST /api/v1/payments
Idempotency-Key: unique-uuid-per-request
{
  "amount": 100,
  "currency": "USD"
}

# Server stores key and result
# Duplicate requests return same response (200 instead of 201)

# Useful for:
- Payment processing
- Order creation
- Any operation where duplicate requests could cause problems
```

---

## HATEOAS (Optional)

### Hypermedia Links

```json
{
  "data": {
    "id": "123",
    "name": "John Doe",
    "status": "active"
  },
  "links": {
    "self": "/api/v1/users/123",
    "orders": "/api/v1/users/123/orders",
    "update": {
      "href": "/api/v1/users/123",
      "method": "PATCH"
    },
    "delete": {
      "href": "/api/v1/users/123",
      "method": "DELETE"
    }
  }
}
```

---

## File Uploads

### Single File

```
POST /api/v1/avatars
Content-Type: multipart/form-data

------WebKitFormBoundary
Content-Disposition: form-data; name="file"; filename="avatar.jpg"
Content-Type: image/jpeg

[binary data]
------WebKitFormBoundary--

# Response
{
  "data": {
    "id": "file-123",
    "url": "https://cdn.example.com/avatars/file-123.jpg",
    "size": 102400,
    "mime_type": "image/jpeg"
  }
}
```

### Large Files (Pre-signed URLs)

```
# 1. Request upload URL
POST /api/v1/uploads/initiate
{
  "filename": "large-video.mp4",
  "content_type": "video/mp4",
  "size": 104857600
}

# 2. Server returns pre-signed URL
{
  "upload_url": "https://s3.amazonaws.com/...",
  "upload_id": "upload-123",
  "expires_at": "2024-03-16T11:30:00Z"
}

# 3. Client uploads directly to S3

# 4. Notify server of completion
POST /api/v1/uploads/upload-123/complete
```

---

## Webhooks

### Webhook Payload

```json
POST https://your-app.com/webhooks/payment
Content-Type: application/json
X-Webhook-Signature: sha256=...

{
  "id": "event-123",
  "type": "payment.succeeded",
  "created_at": "2024-03-16T10:30:00Z",
  "data": {
    "payment_id": "pay-456",
    "amount": 100,
    "status": "succeeded"
  }
}
```

### Security

```
# Include signature for verification
X-Webhook-Signature: sha256=computed-hmac-signature

# Recipient verifies:
const signature = hmac_sha256(payload, webhook_secret);
if (signature !== header_signature) {
  return 401; // Invalid signature
}
```

---

## API Documentation

### OpenAPI/Swagger

```yaml
openapi: 3.0.0
info:
  title: User API
  version: 1.0.0

paths:
  /users:
    get:
      summary: List all users
      parameters:
        - name: page
          in: query
          schema:
            type: integer
            default: 1
        - name: limit
          in: query
          schema:
            type: integer
            default: 20
      responses:
        '200':
          description: Successful response
          content:
            application/json:
              schema:
                type: object
                properties:
                  data:
                    type: array
                    items:
                      $ref: '#/components/schemas/User'

components:
  schemas:
    User:
      type: object
      properties:
        id:
          type: string
        name:
          type: string
        email:
          type: string
          format: email
```

### Documentation Requirements

Every endpoint must document:
- [ ] Description of what it does
- [ ] Authentication requirements
- [ ] Request parameters (path, query, body)
- [ ] Response format (success and errors)
- [ ] Example requests and responses
- [ ] Rate limits
- [ ] Required permissions

---

## Testing APIs

### Test Coverage

```javascript
describe('GET /api/v1/users', () => {
  it('should return 200 with user list', async () => {
    const response = await request(app)
      .get('/api/v1/users')
      .set('Authorization', `Bearer ${validToken}`);

    expect(response.status).toBe(200);
    expect(response.body.data).toBeInstanceOf(Array);
  });

  it('should return 401 without authentication', async () => {
    const response = await request(app).get('/api/v1/users');
    expect(response.status).toBe(401);
  });

  it('should filter by status', async () => {
    const response = await request(app)
      .get('/api/v1/users?status=active')
      .set('Authorization', `Bearer ${validToken}`);

    expect(response.status).toBe(200);
    response.body.data.forEach(user => {
      expect(user.status).toBe('active');
    });
  });

  it('should paginate results', async () => {
    const response = await request(app)
      .get('/api/v1/users?page=1&limit=10')
      .set('Authorization', `Bearer ${validToken}`);

    expect(response.body.data.length).toBeLessThanOrEqual(10);
    expect(response.body.meta).toHaveProperty('total');
  });
});
```

---

## Best Practices Checklist

### Before Releasing an API

- [ ] **Versioned** - URL includes version (v1, v2)
- [ ] **Authenticated** - All endpoints require auth (except public ones)
- [ ] **Authorized** - Permission checks on sensitive operations
- [ ] **Validated** - All inputs validated and sanitized
- [ ] **Consistent** - Naming, structure, errors follow standards
- [ ] **Documented** - OpenAPI spec + examples
- [ ] **Tested** - Unit, integration, and E2E tests
- [ ] **Rate Limited** - Prevents abuse
- [ ] **Monitored** - Logging, metrics, alerts
- [ ] **Error Handling** - Meaningful error messages
- [ ] **Pagination** - Large collections are paginated
- [ ] **HTTPS Only** - No plain HTTP in production
- [ ] **CORS Configured** - Proper origin restrictions

---

## Common Mistakes to Avoid

❌ **Verbs in URLs**
```
Bad:  POST /api/createUser
Good: POST /api/users
```

❌ **Not Using HTTP Methods Correctly**
```
Bad:  GET /api/users/delete/123
Good: DELETE /api/users/123
```

❌ **Exposing Internal IDs**
```
Bad:  "id": 12345 (database auto-increment)
Good: "id": "usr_7k3n9m2p" (UUID or encoded ID)
```

❌ **Inconsistent Naming**
```
Bad:  /users, /product, /orderItems
Good: /users, /products, /order-items
```

❌ **Not Versioning**
```
Bad:  /api/users (what happens when you need to break it?)
Good: /api/v1/users
```

❌ **Leaking Stack Traces in Errors**
```
Bad:  { "error": "Error: Cannot read property 'name' of undefined\n    at..." }
Good: { "error": { "code": "INTERNAL_ERROR", "message": "An error occurred" } }
```

❌ **Not Paginating Large Collections**
```
Bad:  GET /users returns all 1 million users
Good: GET /users?page=1&limit=20
```

---

## Migration Strategy

### Introducing New Standards

1. **Audit Existing APIs** - Document current state
2. **Prioritize** - Which APIs need updates most?
3. **Version New Endpoints** - Don't break existing
4. **Deprecate Gradually** - 6-month sunset period
5. **Communicate** - Notify API consumers
6. **Monitor** - Track usage of old vs new

---

## Resources

- [REST API Tutorial](https://restfulapi.net/)
- [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [HTTP Status Codes](https://httpstatuses.com/)
- [JSON:API](https://jsonapi.org/) - Alternative specification
- Internal API examples: [Link to internal API repo]

---

**Remember**: Great APIs are intuitive, consistent, and well-documented. When in doubt, prioritize developer experience.

**Last Updated**: 2024-03-16
**Next Review**: Quarterly
