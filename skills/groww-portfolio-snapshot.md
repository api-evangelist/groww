---
name: Pull a Groww portfolio snapshot
description: Read holdings, open positions and available margin for the authenticated Groww account.
api: openapi/groww-trade-api-openapi.yml
operations: [createAccessToken, getHoldings, getPositions, getUserMargin]
---

# Pull a Groww portfolio snapshot

Assemble a full account snapshot from the Groww Trading API.

## Rules
- Send `Authorization: Bearer {ACCESS_TOKEN}`, `Accept: application/json`, `X-API-VERSION: 1.0` on every call.
- These are Non-Trading reads: limit 20 req/s, 500 req/min.

## Steps
1. `createAccessToken` — obtain a valid daily token if you do not have one.
2. `getHoldings` — demat holdings (`isin`, `trading_symbol`, `quantity`, `average_price`, pledge/locked breakdown).
3. `getPositions` — open intraday/carry-forward positions with `realised_pnl`.
4. `getUserMargin` — available margin and collateral to size new trades.

## Notes
Combine `getPositions` net quantities with `getUserMargin` before sizing new orders. See conventions/groww-conventions.yml and data-model/groww-data-model.yml.
