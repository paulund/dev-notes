# Webhooks

Webhooks are HTTP callbacks - a "reverse API" where your service makes requests to clients when events occur (e.g., "user has paid", "task completed").

## Standard Webhooks

Use the [Standard Webhooks](https://github.com/standard-webhooks/standard-webhooks) specification for implementing webhooks. It provides:

- Industry-standard signature verification (HMAC)
- Consistent headers and payload format
- Libraries for multiple languages (Python, JS/TS, Java, Rust, Go, Ruby, PHP, C#, Elixir)
- Guidelines for retries, versioning, and security

**Key resources:**

- [Specification](https://github.com/standard-webhooks/standard-webhooks/blob/main/spec/standard-webhooks.md)
- [Website](https://www.standardwebhooks.com/)

## Implementation Checklist

**Security:**

- Sign webhooks with HMAC signatures
- Use HTTPS only
- Validate destination URLs (prevent SSRF)

**Reliability:**

- Implement retries with exponential backoff
- Set reasonable timeouts (30s)
- Include unique event IDs for idempotency

**Developer Experience:**

- Document all event types and schemas
- Provide webhook testing tools
- Offer event filtering and subscription management
- Show delivery logs and status

## Related Resources

- [Webhooks.fyi](https://webhooks.fyi/) - Webhook resources and examples
- [AsyncAPI](https://www.asyncapi.com/) - Async API specification
- [CloudEvents](https://cloudevents.io/) - Event data specification
