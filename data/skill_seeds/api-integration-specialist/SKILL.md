---
name: api-integration-specialist
description: "Expert in integrating third-party APIs with proper authentication, error handling, rate limiting, and retry logic. Use when integrating REST APIs, GraphQL endpoints, webhooks, or external services, implementing OAuth flows, managing API keys, transforming request/response data, building API client libraries, handling pagination, or debugging API integration issues."
---

# API Integration Specialist

Production-ready patterns for integrating external APIs with security best practices and comprehensive error handling.

## Workflow

1. **Assess the API** - Review API docs for auth method, rate limits, pagination, and error codes
2. **Set up authentication** - Configure API keys (env vars) or implement OAuth 2.0 flow
3. **Build the client** - Create a standardized request wrapper with default headers, base URL, and timeout
4. **Add error handling** - Implement structured error types with retry logic for 5xx/429 errors
5. **Add rate limiting** - Implement client-side rate limiter matching API limits
6. **Add response transformation** - Map external API format to internal models
7. **Set up webhooks** (if needed) - Implement signature verification and idempotent event handling
8. **Validate** - Test error scenarios, rate limit behavior, and edge cases before production

## Authentication Patterns

**API Keys**: Store in environment variables, pass via `Authorization: Bearer` header or custom header per API docs.

**OAuth 2.0 Authorization Code Flow**:
```javascript
const oauth = new OAuth2Client({
  clientId: process.env.CLIENT_ID,
  clientSecret: process.env.CLIENT_SECRET,
  redirectUri: process.env.REDIRECT_URI,
  scopes: ['read:users', 'write:data']
});
const authUrl = oauth.getAuthorizationUrl();
const tokens = await oauth.exchangeCode(code);
```

## Core Patterns

### Standardized Request with Error Handling and Retry

```javascript
class APIError extends Error {
  constructor(status, body) {
    super(`API Error: ${status}`);
    this.status = status;
    this.body = body;
  }
  isRetryable() { return this.status === 429 || this.status >= 500; }
}

async function retryWithBackoff(fn, maxRetries = 3) {
  for (let i = 0; i < maxRetries; i++) {
    try { return await fn(); }
    catch (e) {
      if (!e.isRetryable?.() || i === maxRetries - 1) throw e;
      await new Promise(r => setTimeout(r, Math.pow(2, i) * 1000));
    }
  }
}
```

### REST API Client

```javascript
class ServiceAPIClient {
  constructor({ apiKey, baseURL, timeout = 30000 }) {
    this.apiKey = apiKey; this.baseURL = baseURL; this.timeout = timeout;
  }
  async request(method, endpoint, data = null) {
    const opts = { method, headers: { 'Authorization': `Bearer ${this.apiKey}`, 'Content-Type': 'application/json' }, timeout: this.timeout };
    if (data) opts.body = JSON.stringify(data);
    const res = await retryWithBackoff(() => fetch(`${this.baseURL}${endpoint}`, opts));
    if (!res.ok) throw new APIError(res.status, await res.json());
    return res.json();
  }
}
```

### Pagination

```javascript
async function* fetchAllPages(client, endpoint, pageSize = 100) {
  let cursor = null;
  do {
    const params = new URLSearchParams({ limit: pageSize, ...(cursor && { cursor }) });
    const res = await client.request('GET', `${endpoint}?${params}`);
    yield res.data;
    cursor = res.pagination?.next_cursor;
  } while (cursor);
}
```

### Webhook Verification

```javascript
function verifyWebhookSignature(payload, signature, secret) {
  const expected = crypto.createHmac('sha256', secret).update(payload).digest('hex');
  return crypto.timingSafeEqual(Buffer.from(signature), Buffer.from(expected));
}
```

## Best Practices

**Security**: Store keys in env vars or secrets manager. Use HTTPS always. Verify webhook signatures. Rotate keys regularly.

**Reliability**: Exponential backoff retry (only 5xx and 429). Set timeouts (30s default). Use circuit breakers for failing services. Log all API interactions.

**Performance**: Cache responses where appropriate. Batch requests when API supports it. Use streaming for large responses. Implement connection pooling.

**Monitoring**: Track response times, error rates, rate limit consumption. Alert on anomalies. Set up health checks for critical integrations.
