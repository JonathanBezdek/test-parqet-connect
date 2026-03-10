# Parqet Connect OpenAPI Notes (from references/openapi.json)

## API surface

- `GET /user` - basic user/connect info.
- `GET /portfolios` - list portfolios with read access.
- `POST /portfolios` - create portfolio (requires `name`, 1-80 chars).
- `GET /portfolios/{portfolioId}/activities` - list activities with filters.
- `POST /portfolios/{portfolioId}/activities` - create activities (max 100 per request).
- `POST /portfolios/{portfolioId}/holdings/custom` - create custom holding.
- `POST /portfolios/{portfolioId}/quotes/user-managed` - add quotes for custom holdings (max 500).
- `POST /performance` - fetch performance data for up to 25 portfolio IDs.

## Scopes

- `portfolio:read` - read access to portfolio data.
- `portfolio:write` - write access (create portfolios and activities).

## Activities payload

- `activities` array supports up to 100 items.
- Each activity must include `assetIdentifierType` and uses one of three variants:
  - `isin` for securities (stocks, ETFs, bonds) with `isin`.
  - `crypto_symbol` for crypto with `symbol`.
  - `custom_asset` for custom holdings with `holding_id`.
- Each activity includes `shares`, `price`, `currency`, `datetime`, and `type`.

## Activity listing filters

- `limit` (10-500, default 100)
- `cursor` (pagination)
- `activityType` (single or array)
- `assetType` (single or array)
- `holdingId` (single or array)

## Custom holdings

- Create custom holding with `name` and `assetProduct`.
- `assetProduct` enum: `private_equity`, `p2p`, `insurance`, `material_asset`, `other`.

## User-managed quotes

- Require `holdingId` and `quotes`.
- Each quote: `currency`, `datetime`, `price`.
- `quotes` array supports up to 500 items.

## Performance

- Request body requires `portfolioIds` (1-25).
- `interval` supports relative timeframes (`1d`, `1w`, `mtd`, `1m`, `3m`, `6m`, `1y`, `ytd`, `3y`, `5y`, `10y`, `max`) or absolute timeframe (`start`, optional `end`).
