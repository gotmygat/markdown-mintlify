# Solar Sentra API Reference

> Complete API documentation scraped from https://solarsentra.mintlify.app/api-reference/

---

# API Overview

**Base URLs:**
- Production: `https://api.solarsentra.io/v1`
- Sandbox: `https://sandbox.solarsentra.io/v1`

**Authentication:**
- Bearer Token Authentication
- Include JWT token in `Authorization` header
- Format: `Authorization: Bearer YOUR_JWT_TOKEN`

---

# Wallet Tracking

## Track Wallet

> Begin tracking a Solana wallet address for real-time analytics

### Endpoint Details

- **Path**: `/wallets/track`
- **Method**: `POST`
- **Authentication**: Bearer Token (required)

### Request Body

```json
{
  "address": "7xKXtg2CW87d97TXJSDpbD5jBkheTqA83TZRuJosgAsU",
  "alias": "Trading Wallet",
  "alertEnabled": true
}
```

#### Parameters

| Parameter    | Type    | Required | Description                              |
| ------------ | ------- | -------- | ---------------------------------------- |
| address      | string  | Yes      | Solana wallet address to track           |
| alias        | string  | No       | Friendly name for the wallet             |
| alertEnabled | boolean | No       | Enable notifications for wallet activity |

### Response

**Status**: `201 Created`

```json
{
  "walletId": "wlt_abc123def456",
  "address": "7xKXtg2CW87d97TXJSDpbD5jBkheTqA83TZRuJosgAsU",
  "alias": "Trading Wallet",
  "balance": 125.42,
  "tokenAccounts": 23,
  "riskScore": 34,
  "trackedAt": "2023-11-07T05:31:56Z"
}
```

#### Response Fields

| Field         | Type    | Description                           |
| ------------- | ------- | ------------------------------------- |
| walletId      | string  | Unique identifier for tracked wallet  |
| address       | string  | Wallet blockchain address             |
| alias         | string  | User-defined wallet name              |
| balance       | number  | Current SOL balance                   |
| tokenAccounts | integer | Number of SPL token accounts          |
| riskScore     | integer | Risk assessment score (0-100)         |
| trackedAt     | string  | ISO 8601 timestamp of tracking start  |

---

## List Tracked Wallets

> Retrieve all wallets currently being tracked by the user

### Endpoint Details

- **Path**: `/wallets`
- **Method**: `GET`
- **Authentication**: Bearer Token (required)

### Query Parameters

| Parameter | Type   | Default      | Description                          |
| --------- | ------ | ------------ | ------------------------------------ |
| page      | number | 1            | Page number for pagination           |
| limit     | number | 25           | Results per page                     |
| sortBy    | string | lastActivity | Sort field (balance, riskScore, lastActivity, trackedAt) |

### Response

**Status**: `200 OK`

```json
{
  "wallets": [
    {
      "walletId": "wlt_abc123def456",
      "address": "7xKXtg2CW87d97TXJSDpbD5jBkheTqA83TZRuJosgAsU",
      "alias": "Trading Wallet",
      "balance": 125.42,
      "tokenAccounts": 23,
      "nftCount": 5,
      "riskScore": 34,
      "lastActivity": "2023-11-07T05:31:56Z"
    }
  ],
  "total": 47,
  "page": 1,
  "limit": 25
}
```

#### Response Fields

| Field   | Type    | Description                      |
| ------- | ------- | -------------------------------- |
| wallets | array   | Array of wallet objects          |
| total   | integer | Total number of tracked wallets  |
| page    | integer | Current page number              |
| limit   | integer | Number of results per page       |

---

## Get Wallet Details

> Retrieve detailed information about a specific tracked wallet

### Endpoint Details

- **Path**: `/wallets/{walletId}`
- **Method**: `GET`
- **Authentication**: Bearer Token (required)

### Path Parameters

| Parameter | Type   | Required | Description           | Example           |
| --------- | ------ | -------- | --------------------- | ----------------- |
| walletId  | string | Yes      | Wallet identifier     | wlt_abc123def456  |

### Response

**Status**: `200 OK`

```json
{
  "walletId": "wlt_abc123def456",
  "address": "7xKXtg2CW87d97TXJSDpbD5jBkheTqA83TZRuJosgAsU",
  "alias": "Trading Wallet",
  "balance": 125.42,
  "tokenAccounts": [
    {
      "mint": "EPjFWdd5AufqSSqeM2qN1xzybapC8G4wEGGkZwyTDt1v",
      "symbol": "USDC",
      "balance": 1000.5,
      "usdValue": 1000.5
    }
  ],
  "nfts": [
    {
      "mint": "ABC123...",
      "name": "Mad Lads #1234",
      "collection": "Mad Lads",
      "floorPrice": 45.2
    }
  ],
  "riskScore": 34,
  "riskFactors": [
    "high_frequency_trading",
    "interaction_with_mixer"
  ],
  "tags": [
    "trader",
    "defi_user"
  ]
}
```

#### Response Fields

| Field         | Type   | Description                            |
| ------------- | ------ | -------------------------------------- |
| walletId      | string | Wallet unique identifier               |
| address       | string | Blockchain address                     |
| alias         | string | User-defined name                      |
| balance       | number | Current SOL balance                    |
| tokenAccounts | array  | List of SPL token holdings             |
| nfts          | array  | NFT collection details                 |
| riskScore     | number | Risk assessment (0-100)                |
| riskFactors   | array  | Identified risk indicators             |
| tags          | array  | Wallet classification labels           |

---

## Untrack Wallet

> Stop tracking a wallet and remove it from monitoring

### Endpoint Details

- **Path**: `/wallets/{walletId}`
- **Method**: `DELETE`
- **Authentication**: Bearer Token (required)

### Path Parameters

| Parameter | Type   | Required | Description       | Example          |
| --------- | ------ | -------- | ----------------- | ---------------- |
| walletId  | string | Yes      | Wallet identifier | wlt_abc123def456 |

### Response

**Status**: `204 No Content`

No response body. Successful deletion returns status 204.

---

# Transaction Analysis

## Get Transactions

> Retrieve transaction history for a tracked wallet

### Endpoint Details

- **Path**: `/transactions`
- **Method**: `GET`
- **Authentication**: Bearer Token (required)

### Query Parameters

| Parameter | Type   | Required | Default | Description                              |
| --------- | ------ | -------- | ------- | ---------------------------------------- |
| walletId  | string | Yes      | -       | Wallet identifier                        |
| limit     | number | No       | 50      | Number of transactions to return         |
| offset    | number | No       | 0       | Pagination offset                        |
| type      | string | No       | all     | Filter by type (all, transfer, swap, stake, nft) |
| startTime | string | No       | -       | Start timestamp (ISO 8601)               |
| endTime   | string | No       | -       | End timestamp (ISO 8601)                 |

### Response

**Status**: `200 OK`

```json
{
  "transactions": [
    {
      "signature": "5xK2Tg9CW87d97wDnTU5w4a2n3xZp6NV2aJ8r9G4qH3mPzRtYuVb",
      "type": "swap",
      "amount": 10.5,
      "token": "SOL",
      "from": "7xKXtg2CW87d97TXJSDpbD5jBkheTqA83TZRuJosgAsU",
      "to": "9xKXtg2CW87d97TXJSDpbD5jBkheTqA83TZRuJosgAsU",
      "blockTime": "2023-11-07T05:31:56Z",
      "slot": 234567890,
      "fee": 0.000005,
      "status": "success"
    }
  ],
  "total": 347,
  "hasMore": true
}
```

#### Transaction Types

- `transfer` - Token transfer between wallets
- `swap` - Token swap on DEX
- `stake` - Staking operation
- `nft_purchase` - NFT purchase
- `nft_sale` - NFT sale

#### Response Fields

| Field        | Type    | Description                           |
| ------------ | ------- | ------------------------------------- |
| transactions | array   | Array of transaction objects          |
| total        | integer | Total transaction count               |
| hasMore      | boolean | Indicates more results available      |

---

## Get Transaction Analytics

> Retrieve analytical insights about wallet transaction patterns

### Endpoint Details

- **Path**: `/transactions/analytics`
- **Method**: `GET`
- **Authentication**: Bearer Token (required)

### Query Parameters

| Parameter | Type   | Required | Default | Description                          |
| --------- | ------ | -------- | ------- | ------------------------------------ |
| walletId  | string | Yes      | -       | Wallet identifier                    |
| period    | string | No       | 30d     | Time period (24h, 7d, 30d, 90d, 1y)  |

### Response

**Status**: `200 OK`

```json
{
  "totalTransactions": 347,
  "totalVolume": 12847.92,
  "averageTransaction": 37.02,
  "uniqueCounterparties": 89,
  "topTokens": [
    {
      "token": "SOL",
      "transactions": 156,
      "volume": 8943.21
    },
    {
      "token": "USDC",
      "transactions": 98,
      "volume": 2341.87
    }
  ],
  "activityPattern": {
    "mostActiveDay": "Monday",
    "mostActiveHour": 14,
    "averageDailyTxs": 11.6
  }
}
```

#### Response Fields

| Field                 | Type    | Description                              |
| --------------------- | ------- | ---------------------------------------- |
| totalTransactions     | integer | Total transaction count in period        |
| totalVolume           | number  | Total transaction volume (USD)           |
| averageTransaction    | number  | Average transaction size (USD)           |
| uniqueCounterparties  | integer | Number of unique addresses interacted    |
| topTokens             | array   | Most traded tokens with stats            |
| activityPattern       | object  | Activity patterns and trends             |

---

# AI Analytics

## Get AI Predictions

> Retrieve AI-powered predictions for wallet behavior

### Endpoint Details

- **Path**: `/analytics/predictions`
- **Method**: `GET`
- **Authentication**: Bearer Token (required)

### Query Parameters

| Parameter | Type   | Required | Description       |
| --------- | ------ | -------- | ----------------- |
| walletId  | string | Yes      | Wallet identifier |

### Response

**Status**: `200 OK`

```json
{
  "walletId": "wlt_abc123def456",
  "nextTransactionProbability": 0.78,
  "estimatedTimeToNext": 4.2,
  "likelyAction": "swap",
  "confidence": 0.84,
  "predictedTokens": [
    {
      "token": "USDC",
      "probability": 0.65
    },
    {
      "token": "JUP",
      "probability": 0.23
    }
  ],
  "factors": [
    "historical_pattern",
    "time_of_day",
    "recent_market_movement"
  ]
}
```

#### Response Fields

| Field                      | Type   | Description                                   |
| -------------------------- | ------ | --------------------------------------------- |
| walletId                   | string | Wallet identifier                             |
| nextTransactionProbability | number | Probability of next transaction (0-1)         |
| estimatedTimeToNext        | number | Estimated hours until next transaction        |
| likelyAction               | string | Predicted action type                         |
| confidence                 | number | Prediction confidence score (0-1)             |
| predictedTokens            | array  | Likely tokens to be traded                    |
| factors                    | array  | Factors influencing the prediction            |

#### Likely Action Types

- `buy` - Token purchase
- `sell` - Token sale
- `transfer` - Token transfer
- `stake` - Staking operation
- `swap` - Token swap

---

## Get Pattern Recognition

> Identify behavioral patterns in wallet transaction history

### Endpoint Details

- **Path**: `/analytics/patterns`
- **Method**: `GET`
- **Authentication**: Bearer Token (required)

### Query Parameters

| Parameter     | Type   | Required | Default | Description                   |
| ------------- | ------ | -------- | ------- | ----------------------------- |
| walletId      | string | Yes      | -       | Wallet identifier             |
| timeRange     | string | No       | 30d     | Analysis period (7d, 30d, 90d)|
| minConfidence | number | No       | 0.75    | Minimum confidence threshold  |

### Response

**Status**: `200 OK`

```json
{
  "walletId": "wlt_abc123def456",
  "detectedPatterns": [
    {
      "type": "accumulation",
      "confidence": 0.89,
      "startDate": "2023-12-25",
      "tokens": ["SOL", "JUP", "BONK"],
      "averageSize": 150.5,
      "frequency": "2-3 times per day"
    },
    {
      "type": "day_trading",
      "confidence": 0.76,
      "startDate": "2023-12-20",
      "tokens": ["USDC", "SOL"],
      "averageSize": 45.3,
      "frequency": "10-15 times per day"
    }
  ],
  "tradingStyle": "swing_trader",
  "riskProfile": "moderate"
}
```

#### Pattern Types

- `accumulation` - Consistent buying over time
- `distribution` - Gradual selling pattern
- `day_trading` - High-frequency intraday trading
- `swing_trading` - Position holding for days/weeks
- `arbitrage` - Price difference exploitation
- `liquidity_provision` - DEX liquidity providing

#### Trading Styles

- `day_trader` - Very short holding periods
- `swing_trader` - Medium-term positions
- `long_term_holder` - Buy and hold strategy
- `yield_farmer` - DeFi yield optimization

#### Risk Profiles

- `conservative` - Low-risk, stable strategies
- `moderate` - Balanced risk/reward approach
- `aggressive` - High-risk, high-reward strategies

---

## Detect Anomalies

> Detect unusual patterns and potential risks in wallet activity

### Endpoint Details

- **Path**: `/analytics/anomalies`
- **Method**: `GET`
- **Authentication**: Bearer Token (required)

### Query Parameters

| Parameter | Type   | Required | Default | Description                       |
| --------- | ------ | -------- | ------- | --------------------------------- |
| walletId  | string | Yes      | -       | Wallet identifier                 |
| timeRange | string | No       | 7d      | Detection period (24h, 7d, 30d)   |

### Response

**Status**: `200 OK`

```json
{
  "walletId": "wlt_abc123def456",
  "anomalies": [
    {
      "id": "anom_abc123",
      "type": "unusual_volume",
      "severity": "medium",
      "description": "Transaction volume 450% above normal",
      "detectedAt": "2023-11-07T05:31:56Z",
      "affectedTransactions": [
        "5xK2Tg9CW87d97wDnTU5w4a2n3xZp6NV2aJ8r9G4qH3mPzRtYuVb"
      ]
    },
    {
      "id": "anom_def456",
      "type": "suspicious_counterparty",
      "severity": "high",
      "description": "Interaction with flagged high-risk address",
      "detectedAt": "2023-11-06T18:22:10Z",
      "counterparty": "BadAct0r1234567890..."
    }
  ],
  "riskScoreChange": 12,
  "recommendation": "Monitor closely for next 24 hours"
}
```

#### Anomaly Types

- `unusual_volume` - Transaction volume significantly above/below normal
- `irregular_timing` - Activity at unusual times
- `suspicious_counterparty` - Interaction with flagged addresses
- `rapid_transfers` - Unusually quick succession of transfers
- `large_transaction` - Single transaction significantly larger than normal
- `new_token_interaction` - First-time interaction with new tokens

#### Severity Levels

- `low` - Minor deviation from normal behavior
- `medium` - Moderate concern requiring attention
- `high` - Significant risk detected
- `critical` - Immediate action recommended

---

# Risk Assessment

## Get Risk Score

> Calculate and retrieve comprehensive risk assessment for a wallet

### Endpoint Details

- **Path**: `/risk/score`
- **Method**: `GET`
- **Authentication**: Bearer Token (required)

### Query Parameters

| Parameter | Type   | Required | Description       |
| --------- | ------ | -------- | ----------------- |
| walletId  | string | Yes      | Wallet identifier |

### Response

**Status**: `200 OK`

```json
{
  "walletId": "wlt_abc123def456",
  "riskScore": 34,
  "riskLevel": "medium",
  "factors": [
    {
      "factor": "high_frequency_trading",
      "weight": 0.25,
      "score": 45,
      "description": "Wallet executes 50+ transactions daily"
    },
    {
      "factor": "interaction_with_mixer",
      "weight": 0.35,
      "score": 75,
      "description": "Recent interaction with privacy protocol"
    },
    {
      "factor": "new_wallet_age",
      "weight": 0.15,
      "score": 20,
      "description": "Wallet created within last 30 days"
    }
  ],
  "counterpartyRisk": {
    "flaggedInteractions": 3,
    "highRiskCounterparties": [
      "BadAct0r1234567890...",
      "Scammer9876543210..."
    ]
  },
  "complianceFlags": [
    "mixer_interaction",
    "high_value_transfers"
  ],
  "lastUpdated": "2023-11-07T05:31:56Z"
}
```

#### Risk Levels

- `low` (0-25) - Minimal risk, standard activity
- `medium` (26-50) - Moderate risk, some concerning patterns
- `high` (51-75) - Elevated risk, multiple red flags
- `critical` (76-100) - Severe risk, immediate attention required

#### Risk Factors

| Factor                    | Description                                    |
| ------------------------- | ---------------------------------------------- |
| high_frequency_trading    | Unusually high transaction volume              |
| interaction_with_mixer    | Contact with privacy protocols                 |
| new_wallet_age            | Recently created wallet                        |
| large_value_transfers     | Transactions above typical thresholds          |
| geographic_anomaly        | Activity from unusual locations                |
| counterparty_diversity    | Low or high counterparty variation             |
| layering_pattern          | Potential money laundering indicator           |

---

## Create Risk Alert

> Configure custom alerts for risk score changes and suspicious activity

### Endpoint Details

- **Path**: `/risk/alerts`
- **Method**: `POST`
- **Authentication**: Bearer Token (required)

### Request Body

```json
{
  "walletId": "wlt_abc123def456",
  "condition": "risk_score_increase",
  "threshold": 50,
  "notificationMethod": "webhook",
  "webhookUrl": "https://yourapp.com/webhooks/risk-alerts"
}
```

#### Parameters

| Parameter          | Type   | Required | Description                              |
| ------------------ | ------ | -------- | ---------------------------------------- |
| walletId           | string | Yes      | Wallet to monitor                        |
| condition          | string | Yes      | Alert condition trigger                  |
| threshold          | number | Yes      | Threshold value for trigger              |
| notificationMethod | string | Yes      | Notification delivery method             |
| webhookUrl         | string | No       | Webhook URL (required if method=webhook) |

#### Alert Conditions

- `risk_score_increase` - Risk score exceeds threshold
- `unusual_activity` - Anomaly detection triggers
- `large_transfer` - Single transaction exceeds threshold
- `suspicious_counterparty` - Interaction with flagged address

#### Notification Methods

- `email` - Email notification
- `webhook` - HTTP POST to webhook URL
- `sms` - SMS text message

### Response

**Status**: `201 Created`

```json
{
  "alertId": "alert_abc123",
  "walletId": "wlt_abc123def456",
  "condition": "risk_score_increase",
  "threshold": 50,
  "status": "active",
  "createdAt": "2023-11-07T05:31:56Z"
}
```

---

# Data Export

## Export Transactions

> Export wallet transaction data in various formats

### Endpoint Details

- **Path**: `/export/transactions`
- **Method**: `POST`
- **Authentication**: Bearer Token (required)

### Request Body

```json
{
  "walletId": "wlt_abc123def456",
  "format": "csv",
  "startDate": "2023-01-01",
  "endDate": "2023-12-31",
  "includeMetadata": true
}
```

#### Parameters

| Parameter       | Type    | Required | Description                          |
| --------------- | ------- | -------- | ------------------------------------ |
| walletId        | string  | Yes      | Wallet identifier                    |
| format          | string  | Yes      | Export format (json, csv, parquet)   |
| startDate       | string  | No       | Start date (ISO 8601 date)           |
| endDate         | string  | No       | End date (ISO 8601 date)             |
| includeMetadata | boolean | No       | Include transaction metadata         |

#### Export Formats

- `json` - JSON format for programmatic access
- `csv` - CSV format for spreadsheet applications
- `parquet` - Apache Parquet for data warehouses

### Response

**Status**: `201 Created`

```json
{
  "exportId": "exp_abc123",
  "status": "processing",
  "estimatedTime": 45,
  "downloadUrl": "https://api.solarsentra.io/downloads/exp_abc123.csv",
  "expiresAt": "2023-11-08T05:31:56Z"
}
```

#### Response Fields

| Field         | Type    | Description                           |
| ------------- | ------- | ------------------------------------- |
| exportId      | string  | Unique export job identifier          |
| status        | string  | Export status (processing, ready, failed) |
| estimatedTime | integer | Estimated completion time (seconds)   |
| downloadUrl   | string  | Download URL (available when ready)   |
| expiresAt     | string  | Download link expiration timestamp    |

### Export Status Values

- `processing` - Export in progress
- `ready` - Export complete, download available
- `failed` - Export failed, check error details

**Note**: Download URLs are temporary and expire after 24 hours.

---

# Error Responses

All endpoints may return the following error responses:

### 400 Bad Request

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

```json
{
  "error": {
    "code": "authentication_failed",
    "message": "Invalid or expired authentication token"
  }
}
```

### 403 Forbidden

```json
{
  "error": {
    "code": "insufficient_permissions",
    "message": "Your account does not have access to this resource"
  }
}
```

### 404 Not Found

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

```json
{
  "error": {
    "code": "internal_error",
    "message": "An unexpected error occurred",
    "requestId": "req_abc123"
  }
}
```

---

# Rate Limits

API rate limits vary by subscription tier:

| Tier       | Requests/Minute | Requests/Day | WebSocket Connections |
| ---------- | --------------- | ------------ | --------------------- |
| Free       | 100             | 10,000       | 1                     |
| Starter    | 500             | 50,000       | 5                     |
| Pro        | 2,000           | 200,000      | 20                    |
| Enterprise | Custom          | Custom       | Custom                |

Rate limit headers are included in all responses:

```
X-RateLimit-Limit: 100
X-RateLimit-Remaining: 87
X-RateLimit-Reset: 1699344716
```

---

# Pagination

List endpoints support cursor-based pagination:

### Query Parameters

- `page` - Page number (1-indexed)
- `limit` - Results per page (max 100)

### Response

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

---

# Webhooks

Configure webhooks to receive real-time notifications:

### Event Types

| Event                   | Description                    |
| ----------------------- | ------------------------------ |
| `wallet.transaction`    | New transaction detected       |
| `wallet.balance_change` | Wallet balance changed         |
| `wallet.token_update`   | Token account updated          |
| `alert.triggered`       | Custom alert activated         |
| `risk.score_change`     | Risk score updated             |
| `anomaly.detected`      | Anomaly detected               |

### Webhook Payload

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

All webhook requests include:

- `X-Solar-Sentra-Signature` - HMAC SHA256 signature
- `X-Solar-Sentra-Event` - Event type
- `X-Solar-Sentra-Delivery` - Unique delivery ID

Verify signatures to ensure authenticity:

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

---

**End of API Reference Documentation**

_Compiled from https://solarsentra.mintlify.app/ on 2025-10-29_
