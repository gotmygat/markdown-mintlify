# Data Export API

Export wallet transaction data in various formats for analysis and record-keeping.

## Export Transactions

Export wallet transaction data in JSON, CSV, or Parquet formats.

### Endpoint

```
POST /export/transactions
```

### Request

```bash
curl -X POST https://api.solarsentra.io/v1/export/transactions \
  -H "Authorization: Bearer YOUR_JWT_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "walletId": "wlt_abc123def456",
    "format": "csv",
    "startDate": "2023-01-01",
    "endDate": "2023-12-31",
    "includeMetadata": true
  }'
```

### Request Parameters

| Parameter       | Type    | Required | Description                          |
| --------------- | ------- | -------- | ------------------------------------ |
| walletId        | string  | Yes      | Wallet identifier                    |
| format          | string  | Yes      | Export format (json, csv, parquet)   |
| startDate       | string  | No       | Start date (ISO 8601 date)           |
| endDate         | string  | No       | End date (ISO 8601 date)             |
| includeMetadata | boolean | No       | Include transaction metadata         |

### Export Formats

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

### Response Fields

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

## SDK Examples

### TypeScript

```typescript
const exportJob = await client.export.transactions({
  walletId: wallet.walletId,
  format: 'csv',
  startDate: '2023-01-01',
  endDate: '2023-12-31',
  includeMetadata: true
});

// Poll for completion
while (exportJob.status === 'processing') {
  await sleep(5000);
  exportJob = await client.export.getStatus(exportJob.exportId);
}

// Download when ready
if (exportJob.status === 'ready') {
  const data = await fetch(exportJob.downloadUrl);
  console.log('Export complete!');
}
```

### Python

```python
export_job = client.export.transactions(
    wallet_id=wallet.wallet_id,
    format='csv',
    start_date='2023-01-01',
    end_date='2023-12-31',
    include_metadata=True
)

# Wait for completion
while export_job.status == 'processing':
    time.sleep(5)
    export_job = client.export.get_status(export_job.export_id)

# Download when ready
if export_job.status == 'ready':
    response = requests.get(export_job.download_url)
    print('Export complete!')
```

---

## Use Cases

- **Tax Reporting**: Export transactions for tax calculations
- **Compliance**: Generate reports for regulatory requirements
- **Data Analysis**: Import into analytics tools
- **Backup**: Archive transaction history
- **Research**: Academic and market research
