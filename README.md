# allinrekt-agent-tools

Personal, non-commercial tooling for my own Binance Agentic Wallet
(Agent ID: `allinrekt.ai`) on BNB Smart Chain (chainId 56).

The tools use the [Element Market](https://element.market) OpenAPI and the
official `element-js-sdk` to work with NFTs held in my own wallet:

- Read orderbook, collection and asset data (floor price, active listings).
- Create and cancel sell orders (listings) for my own NFTs. Orders are
  EIP-712 signed by the wallet itself through the Binance Agentic Wallet
  CLI (`baw`); the API key is only used for API authentication and is
  never tied to custody of any asset.
- Track claim/mint windows for agent-ID gated drops.

Single user, low request volume, no data redistribution, no third-party
access. This is a hobby project under active development.

## Stack

- Node.js 20+
- `element-js-sdk` v3 (ethers v5)
- Binance Agentic Wallet CLI (`baw`) for transaction previews,
  executions and message signatures

## Usage sketch

```js
const { ElementSDK, Network } = require('element-js-sdk');

const sdk = new ElementSDK({
  networkName: Network.BSC,
  isTestnet: false,
  apiKey: process.env.ELEMENT_API_KEY, // provided by Element
  signer, // ethers v5 signer backed by the agentic wallet
});

// Read the cheapest active sell orders of a collection
const orders = await sdk.queryOrders({
  assetContractAddress: '0x…',
  side: 1,
  orderBy: 'base_price',
  direction: 'asc',
  limit: 20,
});
```

## Status

Work in progress. The read path (orderbook queries, floor tracking) is
implemented; programmatic listing creation is the next step once API
access is granted.

## License

MIT
