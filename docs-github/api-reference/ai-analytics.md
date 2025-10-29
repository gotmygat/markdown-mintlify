# AI Analytics API

AI-powered predictions, pattern recognition, and anomaly detection for wallet behavior.

## Get AI Predictions

Retrieve AI-powered predictions for wallet behavior.

### Endpoint

```
GET /analytics/predictions
```

### Request

```bash
curl -X GET "https://api.solarsentra.io/v1/analytics/predictions?walletId=wlt_abc123def456" \
  -H "Authorization: Bearer YOUR_JWT_TOKEN"
```

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

### Response Fields

| Field                      | Type   | Description                                   |
| -------------------------- | ------ | --------------------------------------------- |
| walletId                   | string | Wallet identifier                             |
| nextTransactionProbability | number | Probability of next transaction (0-1)         |
| estimatedTimeToNext        | number | Estimated hours until next transaction        |
| likelyAction               | string | Predicted action type                         |
| confidence                 | number | Prediction confidence score (0-1)             |
| predictedTokens            | array  | Likely tokens to be traded                    |
| factors                    | array  | Factors influencing the prediction            |

### Likely Action Types

- `buy` - Token purchase
- `sell` - Token sale
- `transfer` - Token transfer
- `stake` - Staking operation
- `swap` - Token swap

---

## Get Pattern Recognition

Identify behavioral patterns in wallet transaction history.

### Endpoint

```
GET /analytics/patterns
```

### Request

```bash
curl -X GET "https://api.solarsentra.io/v1/analytics/patterns?walletId=wlt_abc123def456&timeRange=30d&minConfidence=0.75" \
  -H "Authorization: Bearer YOUR_JWT_TOKEN"
```

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

### Pattern Types

- `accumulation` - Consistent buying over time
- `distribution` - Gradual selling pattern
- `day_trading` - High-frequency intraday trading
- `swing_trading` - Position holding for days/weeks
- `arbitrage` - Price difference exploitation
- `liquidity_provision` - DEX liquidity providing

### Trading Styles

- `day_trader` - Very short holding periods
- `swing_trader` - Medium-term positions
- `long_term_holder` - Buy and hold strategy
- `yield_farmer` - DeFi yield optimization

### Risk Profiles

- `conservative` - Low-risk, stable strategies
- `moderate` - Balanced risk/reward approach
- `aggressive` - High-risk, high-reward strategies

---

## Detect Anomalies

Detect unusual patterns and potential risks in wallet activity.

### Endpoint

```
GET /analytics/anomalies
```

### Request

```bash
curl -X GET "https://api.solarsentra.io/v1/analytics/anomalies?walletId=wlt_abc123def456&timeRange=7d" \
  -H "Authorization: Bearer YOUR_JWT_TOKEN"
```

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

### Anomaly Types

- `unusual_volume` - Transaction volume significantly above/below normal
- `irregular_timing` - Activity at unusual times
- `suspicious_counterparty` - Interaction with flagged addresses
- `rapid_transfers` - Unusually quick succession of transfers
- `large_transaction` - Single transaction significantly larger than normal
- `new_token_interaction` - First-time interaction with new tokens

### Severity Levels

- `low` - Minor deviation from normal behavior
- `medium` - Moderate concern requiring attention
- `high` - Significant risk detected
- `critical` - Immediate action recommended

---

## SDK Examples

### TypeScript

```typescript
import { SolarSentra } from '@solar-sentra/sdk';

const client = new SolarSentra({
  apiKey: process.env.SOLAR_SENTRA_API_KEY
});

// Get AI predictions
const predictions = await client.analytics.getPredictions(walletId);
console.log(`Next transaction in ~${predictions.estimatedTimeToNext} hours`);
console.log(`Confidence: ${predictions.confidence * 100}%`);

// Detect patterns
const patterns = await client.analytics.getPatterns(walletId, {
  timeRange: '30d',
  minConfidence: 0.75
});
console.log(`Trading style: ${patterns.tradingStyle}`);

// Detect anomalies
const anomalies = await client.analytics.getAnomalies(walletId, {
  timeRange: '7d'
});
if (anomalies.anomalies.length > 0) {
  console.log(`Found ${anomalies.anomalies.length} anomalies`);
}
```

### Python

```python
from solar_sentra import SolarSentra

client = SolarSentra(api_key=os.environ['SOLAR_SENTRA_API_KEY'])

# Get AI predictions
predictions = client.analytics.get_predictions(wallet_id)
print(f"Next transaction in ~{predictions.estimated_time_to_next} hours")
print(f"Confidence: {predictions.confidence * 100}%")

# Detect patterns
patterns = client.analytics.get_patterns(
    wallet_id,
    time_range='30d',
    min_confidence=0.75
)
print(f"Trading style: {patterns.trading_style}")

# Detect anomalies
anomalies = client.analytics.get_anomalies(wallet_id, time_range='7d')
if anomalies.anomalies:
    print(f"Found {len(anomalies.anomalies)} anomalies")
```

---

## Related Endpoints

- [Wallet Tracking](wallet-tracking.md) - Track wallets
- [Transaction Analysis](transaction-analysis.md) - Analyze transactions
- [Risk Assessment](risk-assessment.md) - Assess risk scores
