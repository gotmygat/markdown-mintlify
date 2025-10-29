# Wallet Tracking API

Endpoints for tracking and managing Solana wallets.

## Track Wallet

Begin tracking a Solana wallet address for real-time analytics.

### Endpoint

```
POST /wallets/track
```

### Request

```bash
curl -X POST https://api.solarsentra.io/v1/wallets/track \
  -H "Authorization: Bearer YOUR_JWT_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "address": "7xKXtg2CW87d97TXJSDpbD5jBkheTqA83TZRuJosgAsU",
    "alias": "Trading Wallet",
    "alertEnabled": true
  }'
```

### Request Parameters

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

### Response Fields

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

Retrieve all wallets currently being tracked by the user.

### Endpoint

```
GET /wallets
```

### Request

```bash
curl -X GET "https://api.solarsentra.io/v1/wallets?page=1&limit=25&sortBy=lastActivity" \
  -H "Authorization: Bearer YOUR_JWT_TOKEN"
```

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

---

## Get Wallet Details

Retrieve detailed information about a specific tracked wallet.

### Endpoint

```
GET /wallets/{walletId}
```

### Request

```bash
curl -X GET https://api.solarsentra.io/v1/wallets/wlt_abc123def456 \
  -H "Authorization: Bearer YOUR_JWT_TOKEN"
```

### Path Parameters

| Parameter | Type   | Required | Description           |
| --------- | ------ | -------- | --------------------- |
| walletId  | string | Yes      | Wallet identifier     |

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

### Response Fields

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

Stop tracking a wallet and remove it from monitoring.

### Endpoint

```
DELETE /wallets/{walletId}
```

### Request

```bash
curl -X DELETE https://api.solarsentra.io/v1/wallets/wlt_abc123def456 \
  -H "Authorization: Bearer YOUR_JWT_TOKEN"
```

### Path Parameters

| Parameter | Type   | Required | Description       |
| --------- | ------ | -------- | ----------------- |
| walletId  | string | Yes      | Wallet identifier |

### Response

**Status**: `204 No Content`

No response body. Successful deletion returns status 204.

---

## SDK Examples

### TypeScript

```typescript
import { SolarSentra } from '@solar-sentra/sdk';

const client = new SolarSentra({
  apiKey: process.env.SOLAR_SENTRA_API_KEY
});

// Track a wallet
const wallet = await client.wallet.track(
  '7xKXtg2CW87d97TXJSDpbD5jBkheTqA83TZRuJosgAsU',
  { alias: 'My Trading Wallet', alertEnabled: true }
);

// List all tracked wallets
const wallets = await client.wallet.list({ limit: 50 });

// Get wallet details
const details = await client.wallet.get(wallet.walletId);

// Untrack wallet
await client.wallet.untrack(wallet.walletId);
```

### Python

```python
from solar_sentra import SolarSentra

client = SolarSentra(api_key=os.environ['SOLAR_SENTRA_API_KEY'])

# Track a wallet
wallet = client.wallet.track(
    '7xKXtg2CW87d97TXJSDpbD5jBkheTqA83TZRuJosgAsU',
    alias='My Trading Wallet',
    alert_enabled=True
)

# List all tracked wallets
wallets = client.wallet.list(limit=50)

# Get wallet details
details = client.wallet.get(wallet.wallet_id)

# Untrack wallet
client.wallet.untrack(wallet.wallet_id)
```

---

## Related Endpoints

- [Transaction Analysis](transaction-analysis.md) - Get transaction history
- [Risk Assessment](risk-assessment.md) - Calculate risk scores
- [AI Analytics](ai-analytics.md) - Get predictions and patterns
