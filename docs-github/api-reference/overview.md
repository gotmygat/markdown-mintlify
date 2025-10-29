# API Overview

Welcome to the SolarSentra API documentation. This API provides comprehensive access to wallet tracking, transaction analysis, AI analytics, and risk assessment for Solana blockchain.

## Base URLs

- **Production**: `https://api.solarsentra.io/v1`
- **Sandbox**: `https://sandbox.solarsentra.io/v1`

## Authentication

All API requests require authentication using Bearer tokens.

### Authentication Header

```
Authorization: Bearer YOUR_JWT_TOKEN
```

### Example Request

```bash
curl -X GET https://api.solarsentra.io/v1/wallets \
  -H "Authorization: Bearer YOUR_JWT_TOKEN"
```

### Getting Your API Key

1. Sign up at [app.solarsentra.io](https://app.solarsentra.io)
2. Navigate to API Settings
3. Generate a new API key
4. Store it securely (never commit to version control)

## Rate Limits

API rate limits vary by subscription tier:

| Tier       | Requests/Minute | Requests/Day | WebSocket Connections |
| ---------- | --------------- | ------------ | --------------------- |
| Free       | 100             | 10,000       | 1                     |
| Starter    | 500             | 50,000       | 5                     |
| Pro        | 2,000           | 200,000      | 20                    |
| Enterprise | Custom          | Custom       | Custom                |

### Rate Limit Headers

All API responses include rate limit information:

```
X-RateLimit-Limit: 100
X-RateLimit-Remaining: 87
X-RateLimit-Reset: 1699344716
```

## Pagination

List endpoints support cursor-based pagination:

### Query Parameters

- `page` - Page number (1-indexed)
- `limit` - Results per page (max 100)

### Response Format

```json
{
  "data": [...],
  "pagination": {
    "total": 347,
    "page": 2,
    "limit": 25,
    "hasMore": true
  }
}
```

## Error Responses

All endpoints may return the following error responses:

### 400 Bad Request

Invalid parameters or malformed request.

```json
{
  "error": {
    "code": "invalid_parameter",
    "message": "Invalid wallet address format",
    "details": {
      "parameter": "address",
      "reason": "Must be a valid Solana address"
    }
  }
}
```

### 401 Unauthorized

Invalid or expired authentication token.

```json
{
  "error": {
    "code": "authentication_failed",
    "message": "Invalid or expired authentication token"
  }
}
```

### 403 Forbidden

Insufficient permissions for the requested resource.

```json
{
  "error": {
    "code": "insufficient_permissions",
    "message": "Your account does not have access to this resource"
  }
}
```

### 404 Not Found

Requested resource not found.

```json
{
  "error": {
    "code": "resource_not_found",
    "message": "Wallet not found",
    "details": {
      "walletId": "wlt_abc123def456"
    }
  }
}
```

### 429 Too Many Requests

Rate limit exceeded.

```json
{
  "error": {
    "code": "rate_limit_exceeded",
    "message": "API rate limit exceeded",
    "details": {
      "limit": 100,
      "window": "1 minute",
      "retryAfter": 45
    }
  }
}
```

### 500 Internal Server Error

Unexpected server error.

```json
{
  "error": {
    "code": "internal_error",
    "message": "An unexpected error occurred",
    "requestId": "req_abc123"
  }
}
```

## Webhooks

Configure webhooks to receive real-time notifications for wallet events.

### Event Types

| Event                   | Description                    |
| ----------------------- | ------------------------------ |
| `wallet.transaction`    | New transaction detected       |
| `wallet.balance_change` | Wallet balance changed         |
| `wallet.token_update`   | Token account updated          |
| `alert.triggered`       | Custom alert activated         |
| `risk.score_change`     | Risk score updated             |
| `anomaly.detected`      | Anomaly detected               |

### Webhook Payload Example

```json
{
  "event": "wallet.transaction",
  "timestamp": "2023-11-07T05:31:56Z",
  "data": {
    "walletId": "wlt_abc123def456",
    "transaction": {
      "signature": "5xK2Tg9CW87...",
      "type": "swap",
      "amount": 10.5,
      "token": "SOL"
    }
  }
}
```

### Webhook Security

All webhook requests include signature verification headers:

- `X-Solar-Sentra-Signature` - HMAC SHA256 signature
- `X-Solar-Sentra-Event` - Event type
- `X-Solar-Sentra-Delivery` - Unique delivery ID

#### Verifying Webhook Signatures

```javascript
const crypto = require('crypto');

function verifyWebhook(payload, signature, secret) {
  const hmac = crypto.createHmac('sha256', secret);
  const digest = hmac.update(payload).digest('hex');
  return crypto.timingSafeEqual(
    Buffer.from(signature),
    Buffer.from(digest)
  );
}
```

## API Endpoints

### Wallet Tracking
- [Track and manage wallets](wallet-tracking.md)
- Create, read, update, delete wallet tracking
- List all tracked wallets

### Transaction Analysis
- [Get transaction history and analytics](transaction-analysis.md)
- Retrieve transactions with filtering
- Analyze transaction patterns

### AI Analytics
- [AI predictions and pattern recognition](ai-analytics.md)
- Get behavior predictions
- Detect patterns and anomalies

### Risk Assessment
- [Risk scoring and alerts](risk-assessment.md)
- Calculate risk scores
- Configure risk alerts

### Data Export
- [Export data in multiple formats](data-export.md)
- Export to JSON, CSV, Parquet
- Scheduled exports

## SDKs and Tools

### Official SDKs

- **TypeScript/JavaScript**: `npm install @solar-sentra/sdk`
- **Python**: `pip install solar-sentra`

### CLI Tool

```bash
npm install -g @solar-sentra/cli
```

### Code Examples

Check out our [examples repository](https://github.com/solarsentra/examples) for sample implementations.

## Support

Need help with the API?

- **Documentation**: [docs.solarsentra.io](https://docs.solarsentra.io)
- **Discord**: [discord.gg/solarsentra](https://discord.gg/solarsentra)
- **Email**: api-support@solarsentra.io
- **Status**: [status.solarsentra.io](https://status.solarsentra.io)
