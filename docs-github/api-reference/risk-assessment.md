# Risk Assessment API

Calculate risk scores and configure alerts for wallet monitoring.

## Get Risk Score

Calculate and retrieve comprehensive risk assessment for a wallet.

### Endpoint

```
GET /risk/score
```

### Request

```bash
curl -X GET "https://api.solarsentra.io/v1/risk/score?walletId=wlt_abc123def456" \
  -H "Authorization: Bearer YOUR_JWT_TOKEN"
```

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
    }
  ],
  "counterpartyRisk": {
    "flaggedInteractions": 3,
    "highRiskCounterparties": [
      "BadAct0r1234567890..."
    ]
  },
  "complianceFlags": [
    "mixer_interaction",
    "high_value_transfers"
  ],
  "lastUpdated": "2023-11-07T05:31:56Z"
}
```

### Risk Levels

- `low` (0-25) - Minimal risk, standard activity
- `medium` (26-50) - Moderate risk, some concerning patterns
- `high` (51-75) - Elevated risk, multiple red flags
- `critical` (76-100) - Severe risk, immediate attention required

---

## Create Risk Alert

Configure custom alerts for risk score changes and suspicious activity.

### Endpoint

```
POST /risk/alerts
```

### Request

```bash
curl -X POST https://api.solarsentra.io/v1/risk/alerts \
  -H "Authorization: Bearer YOUR_JWT_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "walletId": "wlt_abc123def456",
    "condition": "risk_score_increase",
    "threshold": 50,
    "notificationMethod": "webhook",
    "webhookUrl": "https://yourapp.com/webhooks/risk-alerts"
  }'
```

### Request Parameters

| Parameter          | Type   | Required | Description                              |
| ------------------ | ------ | -------- | ---------------------------------------- |
| walletId           | string | Yes      | Wallet to monitor                        |
| condition          | string | Yes      | Alert condition trigger                  |
| threshold          | number | Yes      | Threshold value for trigger              |
| notificationMethod | string | Yes      | Notification delivery method             |
| webhookUrl         | string | No       | Webhook URL (required if method=webhook) |

### Alert Conditions

- `risk_score_increase` - Risk score exceeds threshold
- `unusual_activity` - Anomaly detection triggers
- `large_transfer` - Single transaction exceeds threshold
- `suspicious_counterparty` - Interaction with flagged address

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

## SDK Examples

### TypeScript

```typescript
const riskData = await client.risk.getScore(walletId);

if (riskData.riskScore > 50) {
  console.log('High risk detected!');

  // Create alert
  await client.risk.createAlert({
    walletId: walletId,
    condition: 'risk_score_increase',
    threshold: 50,
    notificationMethod: 'email'
  });
}
```

### Python

```python
risk_data = client.risk.get_score(wallet_id)

if risk_data.risk_score > 50:
    print('High risk detected!')

    # Create alert
    client.risk.create_alert(
        wallet_id=wallet_id,
        condition='risk_score_increase',
        threshold=50,
        notification_method='email'
    )
```
