# Quickstart

> Get started with Solar Sentra in under 5 minutes

## Prerequisites

Before integrating Solar Sentra, ensure you have:

- Node.js 18.0 or higher
- A Solana wallet address to track
- API key from Solar Sentra dashboard

## Installation

Install the Solar Sentra SDK via npm or yarn:

```bash
npm install @solar-sentra/sdk
```

```bash
yarn add @solar-sentra/sdk
```

## Authentication

Initialize the SDK with your API key:

```typescript
import { SolarSentra } from '@solar-sentra/sdk';

const client = new SolarSentra({
  apiKey: process.env.SOLAR_SENTRA_API_KEY,
  network: 'mainnet-beta'
});
```

**Info**: Store your API key in environment variables. Never commit keys to version control.

## Basic Usage

### Track a Wallet

```typescript
const walletAddress = '7xKXtg2CW87d97TXJSDpbD5jBkheTqA83TZRuJosgAsU';

const walletData = await client.wallet.track(walletAddress);

console.log(walletData);
// Output:
// {
//   address: '7xKXtg2CW87d97TXJSDpbD5jBkheTqA83TZRuJosgAsU',
//   balance: 125.42,
//   tokenAccounts: 23,
//   lastActivity: '2025-10-16T14:21:00Z'
// }
```

### Get Transaction History

```typescript
const transactions = await client.wallet.getTransactions(
  walletAddress,
  { limit: 50, order: 'desc' }
);

transactions.forEach(tx => {
  console.log(`${tx.signature}: ${tx.type} - ${tx.amount} ${tx.token}`);
});
```

## Next Steps

- [Full SDK Reference](sdk-reference.md)
- [API Documentation](../api-reference/overview.md)
- [Developer Tools](tools.md)
