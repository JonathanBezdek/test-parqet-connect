---
name: test-parqet-connect
description: Build, test, or maintain Parqet Connect integrations. Use when implementing OAuth 2.0 authorization-code flow, storing/refreshing tokens, generating clients from the Parqet Connect OpenAPI spec, or syncing portfolios/activities/quotes/performance via the Connect API.
---

# Test Parqet Connect

## Overview

Build Parqet Connect integrations end-to-end: OAuth setup, API client generation, and portfolio/activity sync workflows.

## Workflow

1. Gather integration setup details
2. Implement OAuth 2.0 authorization-code flow
3. Build API client from OpenAPI
4. Implement sync flows
5. Test privately and prepare publish checklist

## 1. Gather integration setup details

- Create an integration in the Developer Console and capture Client ID, scopes, and redirect URIs.
- Choose scopes based on required capabilities (see `references/openapi-notes.md`).
- Determine the Connect API base URL and OAuth endpoints from Parqet docs/Developer Console (not provided in the OpenAPI spec).

## 2. Implement OAuth 2.0 authorization-code flow

- Use an OAuth SDK (Node: `openid-client`, Python: `Authlib`).
- Build an authorization URL with required scopes.
- Handle the redirect, exchange `code` for tokens, and persist tokens per user.
- Implement refresh-token handling and expiry checks.
- Use `state` and PKCE if required by the SDK or integration settings.

## 3. Build API client from OpenAPI

- Prefer generating a typed client from `references/openapi.json` (OpenAPI 3).
- Keep the base URL configurable via environment variables.
- Send `Authorization: Bearer <access_token>` on all requests.
- Use `references/openapi-notes.md` for a concise endpoint list and payload constraints.

## 4. Implement sync flows

- User & portfolio discovery: `GET /user`, `GET /portfolios`.
- Portfolio creation: `POST /portfolios` when needed.
- Activities sync: map broker transactions to activity variants (`isin`, `crypto_symbol`, `custom_asset`).
- Custom holdings: create holding first, then submit activities or user-managed quotes.
- Respect batch limits (`activities` max 100, `quotes` max 500).
- Use cursor pagination for activity listing.

## 5. Test privately and prepare publish checklist

- Authorize the private integration on your own account for end-to-end testing.
- For publishing requirements, follow `references/quickstart.md`.

## References

- `references/openapi.json` - Parqet Connect OpenAPI spec (from https://developer.parqet.com/api-spec/current.json).
- `references/openapi-notes.md` - Condensed endpoint, payload, and limit notes.
- `references/quickstart.md` - Developer Console steps and publishing checklist.
