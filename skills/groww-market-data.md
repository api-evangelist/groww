---
name: Fetch Groww live and historical market data
description: Retrieve LTP, full quotes, OHLC, option chain, greeks and historical candles for instruments.
api: openapi/groww-trade-api-openapi.yml
operations: [createAccessToken, getLtp, getQuote, getOhlc, getOptionChain, getGreeks, getHistoricalCandles]
---

# Fetch Groww live and historical market data

Pull market data from the Groww Trading API for analysis or signals.

## Rules
- Auth headers required on every call (`Authorization: Bearer {ACCESS_TOKEN}`, `X-API-VERSION: 1.0`).
- Live Data limits: 10 req/s, 300 req/min. Batch symbols where possible.
- `getLtp` / `getOhlc` take `exchange_symbols` as a comma-separated list of `EXCHANGE_SYMBOL` tokens.

## Steps
1. `createAccessToken` — ensure a valid token.
2. `getLtp` — last traded price for one or more symbols (cheapest read).
3. `getQuote` — full market quote (`exchange`, `segment`, `trading_symbol`).
4. `getOhlc` — open/high/low/close for the symbols.
5. `getOptionChain` — chain by `exchange` + `underlying` (+ optional `expiry_date`).
6. `getGreeks` — per-contract greeks by exchange/underlying/trading_symbol/expiry.
7. `getHistoricalCandles` — historical range (DEPRECATED; prefer the backtesting historical endpoint).

## Notes
See rate-limits/groww-rate-limits.yml for ceilings and errors/groww-problem-types.yml for error codes.
