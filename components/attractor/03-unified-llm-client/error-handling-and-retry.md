# Error Handling and Retry

The client provides a typed error hierarchy and automatic retry with exponential backoff for transient failures.

## Error Taxonomy

| HTTP Status | Error Class | Retryable | Description |
|-------------|-------------|-----------|-------------|
| 400 | `BadRequestError` | No | Malformed request |
| 401 | `AuthenticationError` | No | Invalid API key |
| 403 | `PermissionError` | No | Insufficient permissions |
| 404 | `NotFoundError` | No | Model or endpoint not found |
| 409 | `ConflictError` | No | Resource conflict |
| 422 | `UnprocessableEntityError` | No | Valid syntax but semantic error |
| 429 | `RateLimitError` | Yes | Rate limit exceeded |
| 500 | `InternalServerError` | Yes | Provider internal error |
| 502 | `BadGatewayError` | Yes | Provider gateway error |
| 503 | `ServiceUnavailableError` | Yes | Provider temporarily unavailable |
| 504 | `GatewayTimeoutError` | Yes | Provider timeout |
| Other | `APIError` | Yes (default) | Unknown status code |

## SDKError Hierarchy

```
SDKError
├── ConfigurationError     # Client misconfiguration (no provider, missing key)
├── APIError               # HTTP-level errors from providers
│   ├── AuthenticationError    # 401
│   ├── PermissionError        # 403
│   ├── NotFoundError          # 404
│   ├── BadRequestError        # 400
│   ├── RateLimitError         # 429
│   ├── InternalServerError    # 500
│   └── ...
├── TimeoutError           # Request timed out
├── ConnectionError        # Network connectivity failure
└── NoObjectGeneratedError # generate_object() failed to produce valid output
```

Each error carries:
- `message`: Human-readable description
- `status_code`: HTTP status (for API errors)
- `retryable`: Whether the client should retry
- `retry_after`: Seconds to wait before retrying (from Retry-After header)
- `provider`: Which provider returned the error

## Exponential Backoff

For retryable errors, the client automatically retries with exponential backoff:

```python
delay = min(base_delay * (2 ** attempt) + jitter, max_delay)
```

| Config | Default | Description |
|--------|---------|-------------|
| `max_retries` | 2 | Maximum retry attempts |
| `base_delay` | 1.0s | Initial delay |
| `max_delay` | 60.0s | Maximum delay cap |
| `jitter` | random 0-1s | Added randomness to prevent thundering herd |

## Retry-After Header Handling

When a provider returns a `Retry-After` header (common with 429 rate limit errors):

1. Parse the header value (seconds or HTTP date)
2. If the value is within `max_delay`, use it as the delay
3. If the value exceeds `max_delay`, use `max_delay` instead
4. The `retry_after` field on the error object is always set for caller visibility

## What's Not Retried

- **Non-retryable errors** (401, 403, 404): Raised immediately
- **Timeouts**: Not retried by default (indicates inherently slow operation)
- **Streaming after partial delivery**: Once stream data has been sent to the caller, retrying would produce duplicate content
- **Per-step, not per-operation**: In multi-step tool loops, retries apply to each individual LLM call, not the entire loop

## Design Decision: Default Retry for Unknown Errors

> Transient failures are more common than permanent ones from unexpected codes. A false retry is cheaper than a false abort.

Unknown HTTP status codes default to `retryable=True`.

## Error Results vs Exceptions

For tool execution errors specifically, the client sends error results to the model rather than raising exceptions:

> Raising on tool failure aborts the entire generation. Sending an error result gives the model the opportunity to retry, use a different tool, or explain the failure.

See [tool-calling.md](tool-calling.md) for details.

## See Also

- [provider-adapters.md](provider-adapters.md) — How providers translate HTTP errors
- [tool-calling.md](tool-calling.md) — Tool execution error handling
- [high-level-api.md](high-level-api.md) — How retries work with high-level functions
