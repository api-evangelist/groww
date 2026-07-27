---
name: Place and track a Groww order
description: Authenticate, check required margin, place an order with an idempotent reference id, then track its status and trades.
api: openapi/groww-trade-api-openapi.yml
operations: [createAccessToken, getOrderMargin, createOrder, getOrderStatus, getOrderTrades, cancelOrder]
---

# Place and track a Groww order

Use the Groww Trading API (`https://api.groww.in`) to place and monitor an order.

## Rules
- Every request needs `Authorization: Bearer {ACCESS_TOKEN}`, `Accept: application/json`, and `X-API-VERSION: 1.0`.
- Access tokens expire daily at 06:00 AM IST — regenerate with `createAccessToken` (API key + secret checksum, or TOTP).
- Segments: `CASH` and `FNO` only (no MCX).
- Idempotency: set a unique `order_reference_id` on `createOrder`. Reusing one returns error `GA007` (duplicate order reference id).

## Steps
1. `createAccessToken` — exchange your API key for an access token (`key_type: approval` with checksum, or `key_type: totp`).
2. `getOrderMargin` — POST the intended order (`trading_symbol`, `transaction_type`, `quantity`, `order_type`, `product`, `exchange`, `segment`) to confirm you have enough margin.
3. `createOrder` — place the order with a fresh `order_reference_id`. Capture the returned `groww_order_id`.
4. `getOrderStatus` — poll status by `groww_order_id` (respect the 20 req/s Non-Trading limit).
5. `getOrderTrades` — once filled, fetch the executions.
6. `cancelOrder` — if still pending and no longer wanted, cancel by `groww_order_id` + `segment`.

## Errors
`GA005` unauthorised (bad/expired token), `GA004` entity not found, `GA007` duplicate reference id. See errors/groww-problem-types.yml.
