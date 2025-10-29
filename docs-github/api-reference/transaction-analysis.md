# Transaction Analysis API

Retrieve and analyze transaction history for tracked wallets.

## Get Transactions

Retrieve transaction history for a tracked wallet.

### Endpoint

```
GET /transactions
```

### Request

```bash
curl -X GET "https://api.solarsentra.io/v1/transactions?walletId=wlt_abc123def456&limit=50&type=all" \
  -H "Authorization: Bearer YOUR_JWT_TOKEN"
```

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

### Transaction Types

- `transfer` - Token transfer between wallets
- `swap` - Token swap on DEX
- `stake` - Staking operation
- `nft_purchase` - NFT purchase
- `nft_sale` - NFT sale

---

## Get Transaction Analytics

Retrieve analytical insights about wallet transaction patterns.

### Endpoint

```
GET /transactions/analytics
```

### Request

```bash
curl -X GET "https://api.solarsentra.io/v1/transactions/analytics?walletId=wlt_abc123def456&period=30d" \
  -H "Authorization: Bearer YOUR_JWT_TOKEN"
```

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

---

## SDK Examples

### TypeScript

```typescript
// Get transactions
const transactions = await client.transactions.get(walletId, {
  limit: 100,
  type: 'swap',
  startTime: '2023-01-01'
});

// Get analytics
const analytics = await client.transactions.getAnalytics(walletId, {
  period: '30d'
});

console.log(`Total Volume: $${analytics.totalVolume}`);
```

### Python

```python
# Get transactions
transactions = client.transactions.get(
    wallet_id,
    limit=100,
    type='swap',
    start_time='2023-01-01'
)

# Get analytics
analytics = client.transactions.get_analytics(wallet_id, period='30d')
print(f"Total Volume: ${analytics.total_volume}")
```
